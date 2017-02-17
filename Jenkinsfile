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
                // bat 'cmake . -DCMAKE_INSTALL_PREFIX=.\\dist'
                bat 'cmake .'
                echo "Building"
                bat 'cmake --build . --config Release --clean-first'
                echo 'Packaging into a zip file'
                bat 'cpack -G ZIP -D CPACK_OUTPUT_FILE_PREFIX=dist'
                // bat 'cmake --build . --target install --config Release'
                archiveArtifacts 'dist/**'

            }
        }
    }


}
