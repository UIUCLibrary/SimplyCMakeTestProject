pipeline {
  agent none

  stages{
    stage("Cloning source") {
       agent any

       steps {
         echo "Cleaning up"
         deleteDir()
         echo "Cloning source"
         checkout scm
         stash includes: '**', name: "Source"
      }

    }

    stage("Windows Configuring Build") {
      agent{
        label 'Windows'
      }

      steps{
        unstash "Source"
        echo "Create build folder"
        bat 'mkdir build'
        echo "Configuring"
        dir('build') {
          bat 'cmake ..'
          stash includes: '**', name: "Windows_configured"
        }
      }

    }

    stage("Windows build") {
      agent{
        label 'Windows'
      }

      steps{
        echo "Cleaning build directory"
        deleteDir()
        unstash "Windows_configured"

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
        echo "Unstashing"
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
