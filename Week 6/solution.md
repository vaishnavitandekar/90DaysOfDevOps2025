# Week 6: Jenkins CI/CD Challenge – Solution

This file documents all the tasks completed as part of Week 6 of the #90DaysOfDevOps challenge, focusing on Jenkins CI/CD, pipelines, agents, RBAC, shared libraries, vulnerability scanning, parameterization, email notifications, and troubleshooting.

---

## Task 1: Create a Jenkins Pipeline Job for CI/CD

**Objective:** Build an end-to-end CI/CD pipeline for a sample application.

**Jenkinsfile:**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
            }
        }
    }
}
````

**Observations:**

* Pipeline ran successfully with clear stage logs.
* No errors encountered.

**Challenges & Fixes:**

* Initially forgot to set `agent any` causing the pipeline to fail; fixed by specifying the agent.

**Interview Notes:**

* Declarative pipelines are simpler and more readable than scripted pipelines.
* Breaking into stages makes CI/CD organized and easier to debug.

---

## Task 2: Multi-Branch Pipeline for Microservices

**Objective:** Create pipelines for multiple microservices in separate branches.

**Steps Taken:**

* Created a multi-branch pipeline job in Jenkins.
* Configured Git repository branch scanning.
* Added parallel stages for different services.

**Jenkinsfile snippet:**

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { steps { echo 'Checking out code...' } }
        stage('Build & Test Services') {
            parallel {
                stage('Service A') { steps { echo 'Building Service A...' } }
                stage('Service B') { steps { echo 'Building Service B...' } }
            }
        }
        stage('Deploy') { steps { echo 'Deploying services...' } }
    }
}
```

**Observations:**

* Multi-branch pipelines allow parallel builds for multiple services.
* Feature branch merge workflow was successfully simulated.

**Interview Notes:**

* Multi-branch pipelines improve CI/CD efficiency for microservices.
* Merging conflicts in multiple branches can be tricky without proper branch management.

---

## Task 3: Configure and Scale Jenkins Agents/Nodes

**Objective:** Distribute build workload across multiple OS environments.

**Steps Taken:**

* Configured two agents: Linux and Windows.
* Assigned labels `linux` and `windows`.
* Modified Jenkinsfile to target correct agent.

**Jenkinsfile snippet:**

```groovy
pipeline {
    agent { label 'linux' }
    stages {
        stage('Build') { steps { echo 'Building on Linux agent...' } }
    }
}
```

**Observations:**

* Jobs ran in parallel on different agents.
* Build speed and reliability improved.

**Interview Notes:**

* Distributed agents reduce bottlenecks and allow parallelism.
* Always label agents properly to avoid misassigned jobs.

---

## Task 4: Implement RBAC in Multi-Team Environment

**Objective:** Secure Jenkins using role-based access control (RBAC).

**Steps Taken:**

* Installed Role Strategy Plugin.
* Created roles: Admin, Developer, Tester.
* Assigned permissions and verified access using test accounts.

**Observations:**

* Developers could run pipelines but not modify roles.
* Admin had full access.

**Interview Notes:**

* RBAC prevents unauthorized access and protects pipelines.
* Weak access control can lead to security issues.

---

## Task 5: Develop and Integrate a Jenkins Shared Library

**Objective:** Reuse common pipeline code across projects.

**Steps Taken:**

* Created a Git repository for shared library.
* Added a function for sending notifications:

```groovy
def sendNotification(String msg) {
    echo "Notification: ${msg}"
}
```

* Integrated library into Jenkinsfile:

```groovy
@Library('my-shared-library') _
pipeline {
    agent any
    stages {
        stage('Notify') {
            steps {
                sendNotification('Pipeline completed successfully')
            }
        }
    }
}
```

**Observations:**

* Reduced code duplication.
* Improved maintainability.

---

## Task 6: Integrate Vulnerability Scanning with Trivy

**Objective:** Ensure Docker images are free from vulnerabilities.

**Steps Taken:**

* Added a `Vulnerability Scan` stage in Jenkinsfile:

```groovy
stage('Vulnerability Scan') {
    steps {
        sh 'trivy image myusername/sample-app:v1.0'
    }
}
```

**Observations:**

* Scan reported minor vulnerabilities.
* No critical issues detected.

**Interview Notes:**

* Automating security checks ensures safer deployments.

---

## Task 7: Dynamic Pipeline Parameterization

**Objective:** Accept runtime parameters for flexible pipelines.

**Jenkinsfile snippet:**

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'TARGET_ENV', defaultValue: 'staging', description: 'Deployment environment')
        string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'Application version')
    }
    stages {
        stage('Build') {
            steps {
                echo "Building ${params.APP_VERSION} for ${params.TARGET_ENV}"
            }
        }
    }
}
```

**Observations:**

* Pipeline behavior changed according to parameter values.
* Useful for multiple environments.

---

## Task 8: Integrate Email Notifications

**Objective:** Notify team on build events.

**Jenkinsfile snippet:**

```groovy
stage('Notify') {
    steps {
        emailext (
            subject: "Build Notification: ${env.JOB_NAME} - #${env.BUILD_NUMBER}",
            body: "Build completed. Check details at: ${env.BUILD_URL}",
            recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        )
    }
}
```

**Observations:**

* Email notifications were received successfully.
* Helped team stay updated on build status.

---

## Task 9: Troubleshooting, Monitoring & Advanced Debugging

**Steps Taken:**

* Introduced errors to simulate pipeline failures.
* Used Jenkins console logs and `docker logs` to debug.
* Added `echo` statements for variable tracking.
* Used Jenkins “Replay” feature to test changes.

**Observations:**

* Troubleshooting workflow became faster.
* Monitoring ensures pipelines stay reliable in production.

---
