pipeline {
    agent {
        label 'ubuntu'
    }
    environment {
        TEST = 'BUILD_TARGETS'
        TOTAL_SETS = 1
        TARGET_SET = 1
        JENKINS_CI = '${WORKSPACE}/.ci'
    }
    stages {
        stage('Install') {
            steps {
                echo 'Building..'
                sh 'rm -rf ${JENKINS_CI};git clone https://github.com/decawave/mynewt-travis-ci ${JENKINS_CI}'
                sh 'chmod +x ${JENKINS_CI}/jenkins/*.sh'
                sh '${JENKINS_CI}/jenkins/linux_jenkins_install.sh'
                sh 'cp -r ${JENKINS_CI}/uwb-apps-project.yml project.yml'
                sh 'mkdir -p targets;cp -r ${JENKINS_CI}/uwb-apps-targets/* targets/'
                echo 'Remove any patches to mynewt-core..'
                sh 'cd repos/apache-mynewt-core;git checkout -- ./;cd -'
                sh 'newt upgrade'
                sh '${JENKINS_CI}/jenkins/uwb-apps-setup.sh'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                sh '${JENKINS_CI}/jenkins/prepare_test.sh ${TOTAL_SETS}'
                sh '${JENKINS_CI}/jenkins/run_test.sh'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing....'
                sh '${JENKINS_CI}/jenkins/post_build.sh'
            }
        }
    }
}