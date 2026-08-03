// SPDX-FileCopyrightText: 2023-2026 Zextras <https://www.zextras.com>
//
// SPDX-License-Identifier: AGPL-3.0-only

library(
    identifier: 'jenkins-lib-common@v4.3.2',
    retriever: modernSCM([
        $class: 'GitSCMSource',
        credentialsId: 'jenkins-integration-with-github-account',
        remote: 'git@github.com:zextras/jenkins-lib-common.git',
    ])
)

properties(defaultPipelineProperties())

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

    stages {
        stage('Setup') {
            steps {
                checkout scm
                gitMetadata()
            }
        }

        stage('Skip CI') {
            steps {
                script { semanticRelease.guard() }
            }
        }

        stage('Security Scan') {
            steps { gitleaksStage() }
        }

        stage('Build') {
            steps {
                echo 'Building deb/rpm packages'
                buildStage([
                    buildFlags: ' -sd ',
                ])
                buildStage([
                    architecture: 'aarch64',
                    buildFlags: ' -sd ',
                    distros: ['ubuntu-jammy'],
                    parallelBuilds: false,
                ])
            }
        }

        stage('Upload artifacts') {
            tools {
                jfrog 'jfrog-cli'
            }
            steps {
                uploadStage()
            }
        }

        stage('Semantic Release') {
            steps {
                semanticRelease()
            }
        }
    }
}
