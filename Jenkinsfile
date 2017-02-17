pipeline {
    agent none

    stages{
      stage("Cleaning") {
        agent any
        steps {
          echo "Cleaning up"
          deleteDir()

        }
      }
      stage("Cloning source") {
        agent any
        steps {
          echo "Cloning source"
          checkout scm
        }

      }
      stage("Windows build") {
        agent{
          label 'Windows'
        }
        steps{

            echo "Configuring"
            bat 'cmake .'

            echo "Building"
            bat 'cmake --build . --config Release --clean-first'
            stash includes: '**', name: "Windows_Binary"
          }
      }

        stage("Windows package") {

          agent{
            label 'Windows'
          }

          steps{
            deleteDir()
            echo "unstashing"
            unstash "Windows_Binary"
            echo 'Packaging into a zip file'
            bat 'cpack -G ZIP -D CPACK_OUTPUT_FILE_PREFIX=dist'

            stash includes: 'dist/**', name: "Windows_packaged"
            }
        }

        stage("Archiving") {
          agent any
          steps {
            echo "Cleaning folder"
            deleteDir()

            echo "unstashing"
            unstash "Windows_packaged"
            archiveArtifacts "**"
          }
        }
    }


}
