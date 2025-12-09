# Customization Guide

📖 **Navigation**: [← README](README.md) | [Branch Strategy](BRANCH_STRATEGY.md) | [Workflow Examples](WORKFLOW_EXAMPLES.md) | **Customization Guide** | [Reference Guide →](REFERENCE_GUIDE.md)

Learn how to adapt the Git-Auto-Release template to different CI/CD platforms and customize the automation pipeline.

---

## Table of Contents

1. [Platform Adaptation](#platform-adaptation)
   - [GitLab CI](#gitlab-ci)
   - [Bitbucket Pipelines](#bitbucket-pipelines)
   - [Jenkins](#jenkins)
   - [Azure DevOps](#azure-devops)
   - [CircleCI](#circleci)
2. [Customizing GitHub Actions Workflow](#customizing-github-actions-workflow)
   - [Adding Custom Pipeline Jobs](#adding-custom-pipeline-jobs)
   - [Modifying Existing Jobs](#modifying-existing-jobs)
   - [Removing Unused Jobs](#removing-unused-jobs)
3. [Version Calculation Logic](#version-calculation-logic)
4. [Release Configuration](#release-configuration)
5. [Branch Strategy Adjustments](#branch-strategy-adjustments)

---

---

## Platform Adaptation

The Git-Auto-Release template uses **GitHub Actions** as the reference implementation. The core concepts can be adapted to any CI/CD platform by implementing the same versioning logic.

### Core Components to Implement

Every platform adaptation needs these components:

1. **Branch Detection** - Identify the source branch of merges/PRs
2. **VERSION File Reading** - Read current version from the VERSION file
3. **Version Calculation** - Apply semantic versioning rules based on branch type
4. **VERSION File Writing** - Update VERSION file with new version
5. **Git Tagging** - Create version tags on appropriate merges
6. **Build Metadata** - Append commit SHA for non-release builds
7. **Pre-release Suffixes** - Add `-alpha`, `-beta`, `-rc.N` as needed

### Version Calculation Rules

```
Branch Pattern          → Version Bump → Example Tag
─────────────────────────────────────────────────────
alpha → main           → MAJOR        → v1.0.0-alpha
beta → main            → MAJOR -beta  → v1.0.0-beta
feature/* → main       → MINOR        → v0.2.0-beta
bugfix/* → main        → PATCH        → v0.1.1-beta
main → release         → Clean        → v1.0.0
hotfix → release       → PATCH        → v1.0.1
```

---

## GitLab CI

Create `.gitlab-ci.yml` in your repository root:

```yaml
variables:
  VERSION_FILE: VERSION
  GIT_STRATEGY: clone
  GIT_DEPTH: 0  # Full history for tagging

stages:
  - calculate-version
  - build
  - test
  - release
  - update-version

# Calculate version based on merge request source branch
calculate-version:
  stage: calculate-version
  script:
    - |
      # Read current VERSION
      CURRENT_VERSION=$(cat $VERSION_FILE)
      IFS='.' read -r MAJOR MINOR PATCH <<< "$CURRENT_VERSION"
      
      # Detect source branch (merge request or direct push)
      if [ -n "$CI_MERGE_REQUEST_SOURCE_BRANCH_NAME" ]; then
        SOURCE_BRANCH="$CI_MERGE_REQUEST_SOURCE_BRANCH_NAME"
        IS_MR=true
      else
        SOURCE_BRANCH="$CI_COMMIT_BRANCH"
        IS_MR=false
      fi
      
      # Remove any pre-release suffix from base version
      MAJOR=$(echo $MAJOR | sed 's/[^0-9]//g')
      MINOR=$(echo $MINOR | sed 's/[^0-9]//g')
      PATCH=$(echo $PATCH | sed 's/[^0-9]//g')
      
      SHORT_SHA=$(echo $CI_COMMIT_SHA | cut -c1-8)
      
      # Calculate version based on branch pattern
      case "$SOURCE_BRANCH" in
        alpha)
          NEXT_MAJOR=$((MAJOR + 1))
          VERSION="${NEXT_MAJOR}.0.0-alpha"
          TAG="v${NEXT_MAJOR}.0.0-alpha"
          ;;
        beta)
          VERSION="${MAJOR}.${MINOR}.${PATCH}-beta"
          TAG="v${MAJOR}.${MINOR}.${PATCH}-beta"
          ;;
        feature/*)
          NEXT_MINOR=$((MINOR + 1))
          VERSION="${MAJOR}.${NEXT_MINOR}.0-beta"
          TAG="v${MAJOR}.${NEXT_MINOR}.0-beta"
          ;;
        bugfix/*|hotfix)
          NEXT_PATCH=$((PATCH + 1))
          if [ "$CI_COMMIT_BRANCH" = "release" ]; then
            VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}"
            TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}"
          else
            VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
            TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
          fi
          ;;
        main)
          # main → release promotion
          VERSION="${MAJOR}.${MINOR}.${PATCH}"
          TAG="v${MAJOR}.${MINOR}.${PATCH}"
          ;;
        *)
          # Development builds
          VERSION="${MAJOR}.${MINOR}.${PATCH}+${SHORT_SHA}"
          TAG=""
          ;;
      esac
      
      echo "VERSION=$VERSION" >> build.env
      echo "TAG=$TAG" >> build.env
      echo "Calculated version: $VERSION"
      echo "Tag to create: $TAG"
  artifacts:
    reports:
      dotenv: build.env
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" || $CI_COMMIT_BRANCH'

# Your build job - customize this section
build:
  stage: build
  needs: [calculate-version]
  script:
    - echo "Building with version $VERSION"
    # Add your build commands here
    # Example: npm install && npm run build
    # Example: mvn clean package -Drevision=$VERSION
    # Example: go build -ldflags "-X main.Version=$VERSION"
  artifacts:
    paths:
      - build/  # Adjust to your build output directory
    expire_in: 1 week
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" || $CI_COMMIT_BRANCH'

# Your test job - customize this section
test:
  stage: test
  needs: [build]
  script:
    - echo "Running tests"
    # Add your test commands here
    # Example: npm test
    # Example: mvn test
    # Example: pytest
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" || $CI_COMMIT_BRANCH'

# Create release and tag (only on main or release branch merges)
release:
  stage: release
  needs: [calculate-version, build, test]
  script:
    - |
      if [ -n "$TAG" ]; then
        echo "Creating tag $TAG"
        git config user.email "ci@gitlab.com"
        git config user.name "GitLab CI"
        git tag -a "$TAG" -m "Release $TAG"
        git push origin "$TAG"
        
        # Create GitLab release
        echo "Creating GitLab release for $TAG"
      else
        echo "No tag to create for this branch"
      fi
  rules:
    - if: '$CI_COMMIT_BRANCH == "main" || $CI_COMMIT_BRANCH == "release"'
      when: on_success

# Update VERSION file after merge
update-version:
  stage: update-version
  needs: [release]
  script:
    - |
      echo "Updating VERSION file to $VERSION"
      echo "$VERSION" > $VERSION_FILE
      git config user.email "ci@gitlab.com"
      git config user.name "GitLab CI"
      git add $VERSION_FILE
      git commit -m "chore: update VERSION to $VERSION [skip ci]"
      git push origin $CI_COMMIT_BRANCH
  rules:
    - if: '$CI_COMMIT_BRANCH == "main" || $CI_COMMIT_BRANCH == "release"'
      when: on_success
```

### GitLab CI Key Points

- **Variables**: Use `CI_MERGE_REQUEST_SOURCE_BRANCH_NAME` for MR source detection
- **Artifacts**: Pass VERSION between jobs using `dotenv` reports
- **Rules**: Control when jobs run using GitLab's rules syntax
- **Tagging**: Ensure `GIT_DEPTH: 0` for full history access

---

## Bitbucket Pipelines

Create `bitbucket-pipelines.yml` in your repository root:

```yaml
image: atlassian/default-image:latest

definitions:
  steps:
    - step: &calculate-version
        name: Calculate Version
        script:
          - |
            # Read current VERSION
            CURRENT_VERSION=$(cat VERSION)
            IFS='.' read -r MAJOR MINOR PATCH <<< "$CURRENT_VERSION"
            
            # Detect branch from Bitbucket variables
            if [ -n "$BITBUCKET_PR_ID" ]; then
              SOURCE_BRANCH="$BITBUCKET_BRANCH"
            else
              SOURCE_BRANCH="$BITBUCKET_BRANCH"
            fi
            
            # Remove pre-release suffixes
            MAJOR=$(echo $MAJOR | sed 's/[^0-9]//g')
            MINOR=$(echo $MINOR | sed 's/[^0-9]//g')
            PATCH=$(echo $PATCH | sed 's/[^0-9]//g')
            
            SHORT_SHA=$(echo $BITBUCKET_COMMIT | cut -c1-8)
            
            # Calculate version (same logic as GitLab)
            case "$SOURCE_BRANCH" in
              alpha)
                NEXT_MAJOR=$((MAJOR + 1))
                VERSION="${NEXT_MAJOR}.0.0-alpha"
                TAG="v${NEXT_MAJOR}.0.0-alpha"
                ;;
              beta)
                VERSION="${MAJOR}.${MINOR}.${PATCH}-beta"
                TAG="v${MAJOR}.${MINOR}.${PATCH}-beta"
                ;;
              feature/*)
                NEXT_MINOR=$((MINOR + 1))
                VERSION="${MAJOR}.${NEXT_MINOR}.0-beta"
                TAG="v${MAJOR}.${NEXT_MINOR}.0-beta"
                ;;
              bugfix/*|hotfix)
                NEXT_PATCH=$((PATCH + 1))
                if [ "$BITBUCKET_BRANCH" = "release" ]; then
                  VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}"
                  TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}"
                else
                  VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
                  TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
                fi
                ;;
              main)
                VERSION="${MAJOR}.${MINOR}.${PATCH}"
                TAG="v${MAJOR}.${MINOR}.${PATCH}"
                ;;
              *)
                VERSION="${MAJOR}.${MINOR}.${PATCH}+${SHORT_SHA}"
                TAG=""
                ;;
            esac
            
            echo "VERSION=$VERSION" >> version.env
            echo "TAG=$TAG" >> version.env
            echo "Version: $VERSION, Tag: $TAG"
        artifacts:
          - version.env
    
    - step: &build
        name: Build
        script:
          - source version.env
          - echo "Building version $VERSION"
          # Add your build commands here
        artifacts:
          - build/**
    
    - step: &test
        name: Test
        script:
          - echo "Running tests"
          # Add your test commands here
    
    - step: &release
        name: Create Release
        script:
          - source version.env
          - |
            if [ -n "$TAG" ]; then
              git config user.email "ci@bitbucket.org"
              git config user.name "Bitbucket Pipelines"
              git tag -a "$TAG" -m "Release $TAG"
              git push origin "$TAG"
            fi
    
    - step: &update-version
        name: Update VERSION File
        script:
          - source version.env
          - echo "$VERSION" > VERSION
          - git config user.email "ci@bitbucket.org"
          - git config user.name "Bitbucket Pipelines"
          - git add VERSION
          - git commit -m "chore: update VERSION to $VERSION [skip ci]"
          - git push origin $BITBUCKET_BRANCH

pipelines:
  pull-requests:
    '**':
      - step: *calculate-version
      - step: *build
      - step: *test
  
  branches:
    main:
      - step: *calculate-version
      - step: *build
      - step: *test
      - step: *release
      - step: *update-version
    
    release:
      - step: *calculate-version
      - step: *build
      - step: *test
      - step: *release
      - step: *update-version
```

### Bitbucket Pipelines Key Points

- **Variables**: Use `$BITBUCKET_BRANCH` and `$BITBUCKET_PR_ID`
- **Artifacts**: Pass data between steps using artifact files
- **Definitions**: Reuse step definitions with YAML anchors
- **Permissions**: Ensure pipeline has write access to repository

---

## Jenkins

Create `Jenkinsfile` in your repository root:

```groovy
pipeline {
    agent any
    
    environment {
        VERSION_FILE = 'VERSION'
        VERSION = ''
        TAG = ''
    }
    
    stages {
        stage('Calculate Version') {
            steps {
                script {
                    // Read current version
                    def currentVersion = readFile(VERSION_FILE).trim()
                    def versionParts = currentVersion.split('\\.')
                    def major = versionParts[0].replaceAll('[^0-9]', '') as Integer
                    def minor = versionParts[1].replaceAll('[^0-9]', '') as Integer
                    def patch = versionParts[2].replaceAll('[^0-9]', '') as Integer
                    
                    // Detect branch
                    def sourceBranch = env.CHANGE_BRANCH ?: env.BRANCH_NAME
                    def shortSha = env.GIT_COMMIT.take(8)
                    
                    // Calculate version based on branch
                    if (sourceBranch == 'alpha') {
                        major = major + 1
                        env.VERSION = "${major}.0.0-alpha"
                        env.TAG = "v${major}.0.0-alpha"
                    } else if (sourceBranch == 'beta') {
                        env.VERSION = "${major}.${minor}.${patch}-beta"
                        env.TAG = "v${major}.${minor}.${patch}-beta"
                    } else if (sourceBranch.startsWith('feature/')) {
                        minor = minor + 1
                        env.VERSION = "${major}.${minor}.0-beta"
                        env.TAG = "v${major}.${minor}.0-beta"
                    } else if (sourceBranch.startsWith('bugfix/') || sourceBranch == 'hotfix') {
                        patch = patch + 1
                        if (env.BRANCH_NAME == 'release') {
                            env.VERSION = "${major}.${minor}.${patch}"
                            env.TAG = "v${major}.${minor}.${patch}"
                        } else {
                            env.VERSION = "${major}.${minor}.${patch}-beta"
                            env.TAG = "v${major}.${minor}.${patch}-beta"
                        }
                    } else if (sourceBranch == 'main') {
                        env.VERSION = "${major}.${minor}.${patch}"
                        env.TAG = "v${major}.${minor}.${patch}"
                    } else {
                        env.VERSION = "${major}.${minor}.${patch}+${shortSha}"
                        env.TAG = ""
                    }
                    
                    echo "Calculated VERSION: ${env.VERSION}"
                    echo "TAG to create: ${env.TAG}"
                }
            }
        }
        
        stage('Build') {
            steps {
                echo "Building with version ${env.VERSION}"
                // Add your build commands here
                // sh 'npm install && npm run build'
                // sh 'mvn clean package'
                // sh 'go build'
            }
        }
        
        stage('Test') {
            steps {
                echo "Running tests"
                // Add your test commands here
                // sh 'npm test'
                // sh 'mvn test'
                // sh 'pytest'
            }
        }
        
        stage('Release') {
            when {
                anyOf {
                    branch 'main'
                    branch 'release'
                }
            }
            steps {
                script {
                    if (env.TAG) {
                        sh """
                            git config user.email 'jenkins@ci.local'
                            git config user.name 'Jenkins CI'
                            git tag -a '${env.TAG}' -m 'Release ${env.TAG}'
                            git push origin '${env.TAG}'
                        """
                    }
                }
            }
        }
        
        stage('Update VERSION') {
            when {
                anyOf {
                    branch 'main'
                    branch 'release'
                }
            }
            steps {
                script {
                    writeFile file: VERSION_FILE, text: env.VERSION
                    sh """
                        git config user.email 'jenkins@ci.local'
                        git config user.name 'Jenkins CI'
                        git add ${VERSION_FILE}
                        git commit -m 'chore: update VERSION to ${env.VERSION} [skip ci]'
                        git push origin ${env.BRANCH_NAME}
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo "Pipeline completed successfully. Version: ${env.VERSION}"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
```

### Jenkins Key Points

- **Branch Detection**: Use `env.CHANGE_BRANCH` for PRs, `env.BRANCH_NAME` for direct pushes
- **Script Block**: Use `script {}` for Groovy logic
- **Credentials**: Configure Git credentials in Jenkins
- **Webhooks**: Set up SCM polling or webhooks for automatic triggers

---

## Azure DevOps

Create `azure-pipelines.yml` in your repository root:

```yaml
trigger:
  branches:
    include:
      - '*'

pr:
  branches:
    include:
      - main
      - alpha
      - beta
      - release

variables:
  VERSION_FILE: 'VERSION'

stages:
  - stage: CalculateVersion
    displayName: 'Calculate Version'
    jobs:
      - job: Version
        displayName: 'Calculate Semantic Version'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - checkout: self
            persistCredentials: true
            fetchDepth: 0
          
          - bash: |
              # Read current version
              CURRENT_VERSION=$(cat $VERSION_FILE)
              IFS='.' read -r MAJOR MINOR PATCH <<< "$CURRENT_VERSION"
              
              # Detect source branch
              if [ -n "$(System.PullRequest.SourceBranch)" ]; then
                SOURCE_BRANCH=$(echo "$(System.PullRequest.SourceBranch)" | sed 's|refs/heads/||')
              else
                SOURCE_BRANCH=$(echo "$(Build.SourceBranch)" | sed 's|refs/heads/||')
              fi
              
              # Remove pre-release suffixes
              MAJOR=$(echo $MAJOR | sed 's/[^0-9]//g')
              MINOR=$(echo $MINOR | sed 's/[^0-9]//g')
              PATCH=$(echo $PATCH | sed 's/[^0-9]//g')
              
              SHORT_SHA=$(echo $(Build.SourceVersion) | cut -c1-8)
              
              # Calculate version
              case "$SOURCE_BRANCH" in
                alpha)
                  NEXT_MAJOR=$((MAJOR + 1))
                  VERSION="${NEXT_MAJOR}.0.0-alpha"
                  TAG="v${NEXT_MAJOR}.0.0-alpha"
                  ;;
                beta)
                  VERSION="${MAJOR}.${MINOR}.${PATCH}-beta"
                  TAG="v${MAJOR}.${MINOR}.${PATCH}-beta"
                  ;;
                feature/*)
                  NEXT_MINOR=$((MINOR + 1))
                  VERSION="${MAJOR}.${NEXT_MINOR}.0-beta"
                  TAG="v${MAJOR}.${NEXT_MINOR}.0-beta"
                  ;;
                bugfix/*|hotfix)
                  NEXT_PATCH=$((PATCH + 1))
                  if [ "$SOURCE_BRANCH" = "release" ]; then
                    VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}"
                    TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}"
                  else
                    VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
                    TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
                  fi
                  ;;
                main)
                  VERSION="${MAJOR}.${MINOR}.${PATCH}"
                  TAG="v${MAJOR}.${MINOR}.${PATCH}"
                  ;;
                *)
                  VERSION="${MAJOR}.${MINOR}.${PATCH}+${SHORT_SHA}"
                  TAG=""
                  ;;
              esac
              
              echo "##vso[task.setvariable variable=VERSION;isOutput=true]$VERSION"
              echo "##vso[task.setvariable variable=TAG;isOutput=true]$TAG"
              echo "Version: $VERSION"
              echo "Tag: $TAG"
            name: calcVersion
            displayName: 'Calculate Version'
  
  - stage: Build
    displayName: 'Build'
    dependsOn: CalculateVersion
    variables:
      VERSION: $[ stageDependencies.CalculateVersion.Version.outputs['calcVersion.VERSION'] ]
    jobs:
      - job: BuildJob
        displayName: 'Build Project'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - bash: |
              echo "Building version $(VERSION)"
              # Add your build commands here
            displayName: 'Build'
  
  - stage: Test
    displayName: 'Test'
    dependsOn: Build
    jobs:
      - job: TestJob
        displayName: 'Run Tests'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - bash: |
              echo "Running tests"
              # Add your test commands here
            displayName: 'Test'
  
  - stage: Release
    displayName: 'Create Release'
    dependsOn: 
      - CalculateVersion
      - Build
      - Test
    condition: and(succeeded(), or(eq(variables['Build.SourceBranch'], 'refs/heads/main'), eq(variables['Build.SourceBranch'], 'refs/heads/release')))
    variables:
      VERSION: $[ stageDependencies.CalculateVersion.Version.outputs['calcVersion.VERSION'] ]
      TAG: $[ stageDependencies.CalculateVersion.Version.outputs['calcVersion.TAG'] ]
    jobs:
      - job: ReleaseJob
        displayName: 'Create Tag and Release'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - checkout: self
            persistCredentials: true
          
          - bash: |
              if [ -n "$(TAG)" ]; then
                git config user.email "azuredevops@ci.local"
                git config user.name "Azure DevOps"
                git tag -a "$(TAG)" -m "Release $(TAG)"
                git push origin "$(TAG)"
                
                echo "$(VERSION)" > $(VERSION_FILE)
                git add $(VERSION_FILE)
                git commit -m "chore: update VERSION to $(VERSION) [skip ci]"
                git push origin HEAD:$(Build.SourceBranchName)
              fi
            displayName: 'Create Tag and Update VERSION'
```

### Azure DevOps Key Points

- **Variables**: Use `System.PullRequest.SourceBranch` and `Build.SourceBranch`
- **Output Variables**: Use `##vso[task.setvariable]` to pass data between stages
- **Persistence**: Use `persistCredentials: true` for git operations
- **Conditions**: Control stage execution with `condition` expressions

---

## CircleCI

Create `.circleci/config.yml` in your repository:

```yaml
version: 2.1

executors:
  default:
    docker:
      - image: cimg/base:stable
    working_directory: ~/project

commands:
  calculate-version:
    steps:
      - run:
          name: Calculate Version
          command: |
            # Read current version
            CURRENT_VERSION=$(cat VERSION)
            IFS='.' read -r MAJOR MINOR PATCH <<< "$CURRENT_VERSION"
            
            # Detect branch
            if [ -n "$CIRCLE_PR_NUMBER" ]; then
              SOURCE_BRANCH="$CIRCLE_BRANCH"
            else
              SOURCE_BRANCH="$CIRCLE_BRANCH"
            fi
            
            # Remove pre-release suffixes
            MAJOR=$(echo $MAJOR | sed 's/[^0-9]//g')
            MINOR=$(echo $MINOR | sed 's/[^0-9]//g')
            PATCH=$(echo $PATCH | sed 's/[^0-9]//g')
            
            SHORT_SHA=$(echo $CIRCLE_SHA1 | cut -c1-8)
            
            # Calculate version
            case "$SOURCE_BRANCH" in
              alpha)
                NEXT_MAJOR=$((MAJOR + 1))
                VERSION="${NEXT_MAJOR}.0.0-alpha"
                TAG="v${NEXT_MAJOR}.0.0-alpha"
                ;;
              beta)
                VERSION="${MAJOR}.${MINOR}.${PATCH}-beta"
                TAG="v${MAJOR}.${MINOR}.${PATCH}-beta"
                ;;
              feature/*)
                NEXT_MINOR=$((MINOR + 1))
                VERSION="${MAJOR}.${NEXT_MINOR}.0-beta"
                TAG="v${MAJOR}.${NEXT_MINOR}.0-beta"
                ;;
              bugfix/*|hotfix)
                NEXT_PATCH=$((PATCH + 1))
                if [ "$SOURCE_BRANCH" = "release" ]; then
                  VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}"
                  TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}"
                else
                  VERSION="${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
                  TAG="v${MAJOR}.${MINOR}.${NEXT_PATCH}-beta"
                fi
                ;;
              main)
                VERSION="${MAJOR}.${MINOR}.${PATCH}"
                TAG="v${MAJOR}.${MINOR}.${PATCH}"
                ;;
              *)
                VERSION="${MAJOR}.${MINOR}.${PATCH}+${SHORT_SHA}"
                TAG=""
                ;;
            esac
            
            echo "export VERSION=$VERSION" >> $BASH_ENV
            echo "export TAG=$TAG" >> $BASH_ENV
            echo "Version: $VERSION, Tag: $TAG"

jobs:
  version:
    executor: default
    steps:
      - checkout
      - calculate-version
      - persist_to_workspace:
          root: ~/project
          paths:
            - .
  
  build:
    executor: default
    steps:
      - attach_workspace:
          at: ~/project
      - run:
          name: Build
          command: |
            source $BASH_ENV
            echo "Building version $VERSION"
            # Add your build commands here
      - persist_to_workspace:
          root: ~/project
          paths:
            - .
  
  test:
    executor: default
    steps:
      - attach_workspace:
          at: ~/project
      - run:
          name: Test
          command: |
            echo "Running tests"
            # Add your test commands here
  
  release:
    executor: default
    steps:
      - attach_workspace:
          at: ~/project
      - run:
          name: Create Release
          command: |
            source $BASH_ENV
            if [ -n "$TAG" ]; then
              git config user.email "circleci@ci.local"
              git config user.name "CircleCI"
              git tag -a "$TAG" -m "Release $TAG"
              git push origin "$TAG"
              
              echo "$VERSION" > VERSION
              git add VERSION
              git commit -m "chore: update VERSION to $VERSION [skip ci]"
              git push origin $CIRCLE_BRANCH
            fi

workflows:
  version: 2
  build-and-release:
    jobs:
      - version
      - build:
          requires:
            - version
      - test:
          requires:
            - build
      - release:
          requires:
            - version
            - build
            - test
          filters:
            branches:
              only:
                - main
                - release
```

### CircleCI Key Points

- **Variables**: Use `$CIRCLE_BRANCH` and `$CIRCLE_PR_NUMBER`
- **Environment**: Use `$BASH_ENV` to persist environment variables
- **Workspaces**: Share data between jobs using workspaces
- **Filters**: Control job execution with workflow filters

---
---

## Customizing GitHub Actions Workflow

The `.github/workflows/ci-cd-versioned.yml` file contains the complete automation pipeline. This section explains how to customize it for your needs.

### Workflow Structure

The workflow consists of independent jobs:

1. **`version`** - Calculates semantic version (⚠️ **Core - Do Not Modify**)
2. **`build`** - **Customize this** with your build/test steps
3. **`docker`** - Optional Docker image building
4. **`release`** - Creates tags and releases (⚠️ **Core - Do Not Modify**)
5. **`update_version`** - Updates VERSION file (⚠️ **Core - Do Not Modify**)
6. **`sync_branches`** - Syncs branches after release (⚠️ **Core - Do Not Modify**)

### Adding Custom Pipeline Jobs

#### Step 1: Define Your Job

Add a new job after the `build` job in the workflow file:

```yaml
your-custom-job:
  name: Your Custom Job Name
  runs-on: ubuntu-latest  # or: windows-latest, macos-latest
  needs: [version, build]  # Jobs this depends on
  if: github.event_name == 'push' && github.ref_name == 'main'  # When to run
  steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Your custom step
      run: |
        echo "Running custom logic"
        echo "Version: ${{ needs.version.outputs.version }}"
        # Add your commands here
```

#### Step 2: Add Job Dependencies

Update other jobs that should depend on your new job:

```yaml
release:
  name: Create Release
  needs: [version, build, your-custom-job]  # Added your-custom-job
```

#### Step 3: Access Version Information

Use outputs from the `version` job:

```yaml
- name: Use version
  run: |
    echo "VERSION: ${{ needs.version.outputs.version }}"
    echo "TAG: ${{ needs.version.outputs.tag }}"
    echo "IS_RELEASE: ${{ needs.version.outputs.is_release }}"
    echo "SHORT_SHA: ${{ needs.version.outputs.short_sha }}"
```

### Modifying Existing Jobs

#### Customizing the Build Job

The `build` job is **the only job you should modify** for your project's build commands:

```yaml
build:
  name: Build and Test
  runs-on: ubuntu-latest
  needs: version
  
  steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    # MODIFY THIS SECTION - Add your setup steps
    - name: Setup your environment
      run: |
        # Install dependencies
        # Example for Node.js: npm ci
        # Example for Python: pip install -r requirements.txt
        # Example for Go: go mod download
    
    # MODIFY THIS SECTION - Add your build commands
    - name: Build
      run: |
        # Your build commands
        # Pass version as needed: export VERSION=${{ needs.version.outputs.version }}
      env:
        VERSION: ${{ needs.version.outputs.version }}
    
    # MODIFY THIS SECTION - Add your test commands
    - name: Test
      run: |
        # Your test commands
        # Example: npm test
        # Example: pytest
        # Example: go test ./...
    
    # OPTIONAL - Upload build artifacts
    - name: Upload artifacts
      uses: actions/upload-artifact@v4
      with:
        name: build-artifacts
        path: |
          dist/
          build/
          # Adjust paths to your build output
```

#### Adding Environment Variables

Add environment variables at the job or workflow level:

```yaml
# Workflow level (available to all jobs)
env:
  NODE_ENV: production
  MY_CUSTOM_VAR: value

# Job level (available to one job)
build:
  name: Build
  runs-on: ubuntu-latest
  env:
    BUILD_CONFIG: release
  steps:
    # ...
```

#### Using Secrets

Reference GitHub secrets in your workflow:

```yaml
- name: Deploy
  run: ./deploy.sh
  env:
    API_KEY: ${{ secrets.MY_API_KEY }}
    DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

**Note**: Add secrets in GitHub → Settings → Secrets and variables → Actions

### Removing Unused Jobs

#### Remove Docker Job

If you don't use Docker:

1. **Delete the entire `docker` job** (lines ~340-425)
2. **Update the `release` job** to remove the docker dependency:

```yaml
release:
  name: Create Release
  runs-on: ubuntu-latest
  needs: [version, build]  # Removed 'docker'
  if: github.ref_name == 'release' && github.event_name == 'push'
```

#### Remove Branch Sync Job

If you don't want automatic branch syncing after releases:

1. **Delete the entire `sync_branches` job**
2. No other changes needed (it's the last job in the chain)

### Common Pipeline Additions

#### Add Code Linting

```yaml
lint:
  name: Lint Code
  runs-on: ubuntu-latest
  steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Run linter
      run: |
        # Your linting commands
        # Example: npm run lint
        # Example: flake8 .
        # Example: golangci-lint run

# Update build job to depend on lint
build:
  needs: [version, lint]
```

#### Add Security Scanning

```yaml
security:
  name: Security Scan
  runs-on: ubuntu-latest
  steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Run security scanner
      run: |
        # Your security scanning commands
        # Example: npm audit
        # Example: safety check
        # Example: gosec ./...
```

#### Add Code Coverage

```yaml
- name: Generate coverage report
  run: |
    # Your coverage commands
    # Example: npm run test:coverage
    # Example: pytest --cov
    # Example: go test -coverprofile=coverage.out

- name: Upload coverage to Codecov
  uses: codecov/codecov-action@v3
  with:
    file: ./coverage.xml  # Adjust to your coverage file
    fail_ci_if_error: true
```

#### Add Notifications

```yaml
notify:
  name: Send Notifications
  runs-on: ubuntu-latest
  needs: [release]
  if: always()  # Run even if previous jobs fail
  steps:
    - name: Notify Slack
      uses: 8398a7/action-slack@v3
      with:
        status: ${{ job.status }}
        text: 'Release ${{ needs.version.outputs.tag }} completed'
        webhook_url: ${{ secrets.SLACK_WEBHOOK }}
    
    - name: Notify Email
      uses: dawidd6/action-send-mail@v3
      with:
        server_address: smtp.gmail.com
        server_port: 465
        username: ${{ secrets.EMAIL_USERNAME }}
        password: ${{ secrets.EMAIL_PASSWORD }}
        subject: Release ${{ needs.version.outputs.tag }}
        body: Build job ${{ job.status }}
        to: team@example.com
```

---

## Version Calculation Logic

⚠️ **Warning**: Modifying version calculation can break semantic versioning. Only change if you understand the implications.

### Current Version Logic Location

The version calculation is in the `version` job, around lines 40-180 in `.github/workflows/ci-cd-versioned.yml`.

### Customizing Version Bump Rules

#### Change MAJOR Bump Source

Current: `alpha` → `main` triggers MAJOR bump

To use a different branch pattern:

```bash
# Find this section in the version job:
case "$PR_SOURCE" in
  alpha)
    # Change to your pattern
    # Example: use breaking-change/* branches instead
  breaking-change/*)
    NEXT_MAJOR=$((MAJOR + 1))
    VERSION="${NEXT_MAJOR}.0.0-alpha"
    TAG="v${NEXT_MAJOR}.0.0-alpha"
    ;;
```

#### Add Custom Branch Patterns

Example: Add `enhancement/*` branches with MINOR bumps:

```bash
enhancement/*)
  if [ "${{ github.event_name }}" = "pull_request" ]; then
    NEXT_MINOR=$((MINOR + 1))
    VERSION="${MAJOR}.${NEXT_MINOR}.0-beta"
    TAG=""
  else
    VERSION="${MAJOR}.${MINOR}.${PATCH}+${SHORT_SHA}"
    TAG=""
  fi
  IS_RELEASE="false"
  ;;
```

#### Modify Pre-release Suffixes

Current: Uses `-alpha`, `-beta`, `-rc.N`

To add custom suffix:

```bash
alpha)
  NEXT_MAJOR=$((MAJOR + 1))
  VERSION="${NEXT_MAJOR}.0.0-preview"  # Changed from -alpha
  TAG="v${NEXT_MAJOR}.0.0-preview"
  ;;
```

#### Change Build Metadata Format

Current: Uses `+SHA` (e.g., `v1.0.0+abc123`)

To add custom metadata:

```bash
VERSION="${MAJOR}.${MINOR}.${PATCH}+build.${GITHUB_RUN_NUMBER}.${SHORT_SHA}"
```

---

## Release Configuration

### Customizing Release Notes

Edit the `release` job to customize GitHub release body:

```yaml
- name: Create GitHub Release
  uses: softprops/action-gh-release@v1
  with:
    tag_name: ${{ needs.version.outputs.tag }}
    name: Release ${{ needs.version.outputs.tag }}
    body: |
      ## Release ${{ needs.version.outputs.tag }}
      
      **Date**: ${{ github.event.head_commit.timestamp }}
      **Commit**: ${{ github.sha }}
      
      ### Changes
      
      <!-- Add custom release notes sections -->
      
      ### Installation
      
      ```bash
      # Add installation instructions
      ```
      
      ### Documentation
      
      [View Docs](https://your-docs-url.com)
      
      ---
      
      **Full Changelog**: https://github.com/${{ github.repository }}/compare/v0.1.0...${{ needs.version.outputs.tag }}
    draft: false
    prerelease: false
    generate_release_notes: true  # Auto-generate from commits
```

### Adding Release Assets

Upload build artifacts to releases:

```yaml
- name: Build release artifacts
  run: |
    # Build your distributable files
    tar -czf release.tar.gz dist/

- name: Create GitHub Release
  uses: softprops/action-gh-release@v1
  with:
    tag_name: ${{ needs.version.outputs.tag }}
    files: |
      release.tar.gz
      dist/**/*.zip
      dist/**/*.exe
      # Add your artifact paths
```

### Conditional Release Creation

Only create releases for production tags:

```yaml
- name: Create GitHub Release
  if: ${{ !contains(needs.version.outputs.tag, 'beta') && !contains(needs.version.outputs.tag, 'alpha') }}
  uses: softprops/action-gh-release@v1
```

---

## Branch Strategy Adjustments

### Simplifying Branch Structure

#### Remove Beta Branch (3-Tier Model)

If you don't need beta testing:

1. **Update version job** - Remove beta case logic
2. **Update branch strategy docs** - Remove beta references
3. **Simplify to**: `feature/* → main → release`

#### Remove Alpha Branch (2-Tier Model)

For simple projects:

1. **Keep only**: `main` (staging) and `release` (production)
2. **All features merge to**: `main` with `-beta` suffix
3. **Promote to production**: `main → release` removes suffix

### Adding Custom Branches

#### Add Staging Branch

Insert a staging branch between main and release:

```yaml
# Add to triggers
on:
  push:
    branches:
      - staging  # Add this

# Add staging logic to version job
staging)
  VERSION="${MAJOR}.${MINOR}.${PATCH}-staging+${SHORT_SHA}"
  TAG=""
  IS_RELEASE="false"
  ;;
```

#### Add Release Candidate Branches

Use `rc/*` branches for final testing:

```yaml
rc/*)
  VERSION="${MAJOR}.${MINOR}.${PATCH}-rc.${PR_NUMBER}"
  TAG=""
  IS_RELEASE="false"
  ;;
```

---

## Best Practices

### Do's ✅

- **Customize only the `build` job** for your build commands
- **Add new jobs** for additional pipeline steps (lint, security, deploy)
- **Use job dependencies** (`needs:`) to control execution order
- **Test changes** in a separate branch before applying to main
- **Document customizations** in your project's README

### Don'ts ❌

- **Don't modify** the `version` job unless absolutely necessary
- **Don't modify** the `release`, `update_version`, or `sync_branches` jobs
- **Don't hardcode** version numbers - always use `${{ needs.version.outputs.version }}`
- **Don't skip** the VERSION file update process
- **Don't change** branch naming patterns without updating all logic

---

## Troubleshooting

### Version Not Updating

**Issue**: VERSION file not updating after merge

**Solution**: Check that `sync_branches` job has write permissions:
```yaml
permissions:
  contents: write
```

### Tags Not Creating

**Issue**: Tags not appearing after release merge

**Solution**: Ensure repository settings allow GitHub Actions to create tags

### Build Failing on Custom Commands

**Issue**: Your build commands fail in CI but work locally

**Solution**: 
1. Check environment setup matches local environment
2. Add missing dependencies to the build job
3. Verify paths and working directory
4. Check environment variables are passed correctly

---

**Need help with customization?** Open an issue with your use case!
