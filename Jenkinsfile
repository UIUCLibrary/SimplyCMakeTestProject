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
                echo "Configuring"
                bat 'cmake . -DCMAKE_INSTALL_PREFIX=.\\dist'
                echo "Building"
                bat 'cmake --build . --config Release --clean-first'
                // bat 'cmake --build . --target install --config Release'
                archiveArtifacts 'dist/**'

            }
        }
    }


}
