pipeline {
    agent none

    stages{
        stage("Windows build") {
            agent{
                label 'Windows'
            }
            steps{
              echo "Cleaning up"
              deleteDir()
              echo "Cloning source"
              checkout scm
              echo "Configuring"
              bat 'cmake .'
              echo "Building"
              bat 'cmake --build . --config Release --clean-first'
              echo 'Packaging into a zip file'
              bat 'cpack -G ZIP -D CPACK_OUTPUT_FILE_PREFIX=dist'
              archiveArtifacts 'dist/**'

            }
        }
    }


}
