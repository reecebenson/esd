pipeline {
    agent {
        label 'master'
    }
    options {
        disableConcurrentBuilds()
    }
    stages {
        stage('lint') {
            steps {
                sh './gradlew check'
            }
            post {
                always {
                    script {
                        publishHTML target: [
                                allowMissing         : false,
                                alwaysLinkToLastBuild: false,
                                keepAll              : true,
                                reportDir            : 'build/reports/checkstyle/',
                                reportFiles          : 'main.html,test.html',
                                reportName           : 'checkstyle',
                        ]
                        publishHTML target: [
                                allowMissing         : false,
                                alwaysLinkToLastBuild: false,
                                keepAll              : true,
                                reportDir            : 'build/reports/pmd/',
                                reportFiles          : 'main.html,test.html',
                                reportName           : 'pmd',
                        ]
                        publishHTML target: [
                                allowMissing         : false,
                                alwaysLinkToLastBuild: false,
                                keepAll              : true,
                                reportDir            : 'build/reports/spotbugs/',
                                reportFiles          : 'main.html,test.html',
                                reportName           : 'spotbugs',
                        ]
                    }
                }
            }
        }
        stage('test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('build') {
            steps {
                sh './gradlew autodeploy'
            }
        }
        stage('deploy') {
            when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
            steps {
                sh "ssh billy@esd.novucs.net ./esd/cicd/redeploy.sh ${pwd()} latest"
            }
        }
    }
}