pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "./gradlew clean build"
            }
        }
        stage('Test') {
            steps {
                sh "./gradlew clean test --refresh-dependencies"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        stage("Code Quality") {
                env.JAVA_HOME="${tool 'JDK16'}"
                env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
                def scannerHome = tool 'SonarQube Scanner'

                withSonarQubeEnv('SonarCloud') {
                    if (env.CHANGE_ID != null) {
                        sh "${scannerHome}/bin/sonar-scanner \
                        -Dproject.settings=sonar-project.properties \
                        -Dsonar.projectBaseDir=. \
                        -Dsonar.pullrequest.provider=github \
                        -Dsonar.pullrequest.branch=${env.CHANGE_BRANCH} \
                        -Dsonar.pullrequest.key=${env.CHANGE_ID} \
                        -Dsonar.pullrequest.base=${env.CHANGE_TARGET}"
                    } else {
                        sh "${scannerHome}/bin/sonar-scanner \
                        -Dproject.settings=sonar-project.properties \
                        -Dsonar.projectBaseDir=. \
                        -Dsonar.branch.name=${branch}"
                    }
                }
    }
}