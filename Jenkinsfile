library(
    identifier: 'jenkins-packages-build-library@1.0.4',
    retriever: modernSCM([
        $class: 'GitSCMSource',
        remote: 'git@github.com:zextras/jenkins-packages-build-library.git',
        credentialsId: 'jenkins-integration-with-github-account'
    ])
)

pipeline {
    agent {
        node {
            label 'base'
        }
    }

    options {
        skipDefaultCheckout()
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 1, unit: 'HOURS')
    }

    parameters {
        booleanParam defaultValue: false,
            description: 'Upload packages in playground repositories.',
            name: 'PLAYGROUND'
    }

    tools {
        jfrog 'jfrog-cli'
    }

    stages {
        stage('Checkout & Stash') {
            steps {
                checkout scm
                script {
                    gitMetadata()
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building deb/rpm packages'
                buildStage([
                    ubuntuSinglePkg: true,
                    rockySinglePkg: true,
                ])
            }
        }

        stage('Upload artifacts')
        {
            steps {
                uploadStage(
                    packages: yapHelper.getPackageNames(),
                    ubuntuSinglePkg: true,
                    rockySinglePkg: true,
                )
            }
        }
    }
}

