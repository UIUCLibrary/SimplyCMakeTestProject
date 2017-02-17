pipeline {
    agent none

    stages{
        stage("Windows build") {
            agent{
                label 'Windows'
            }
            steps{
                echo "Cloning source"
                checkout scm
                echo "Building"
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/UIUCLibrary/SimplyCMakeTestProject.git']]])
                echo "Configuring"
                bat 'cmake . -DCMAKE_INSTALL_PREFIX=.\\dist'
                bat 'cmake --build . --config Release --clean-first'
                bat 'cmake --build . --target install --config Release'
                archiveArtifacts 'dist/**'

            }
        }
    }


}
