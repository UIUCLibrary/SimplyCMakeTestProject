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

    stage("Windows Building 32 bit") {
      agent{
        label 'Windows'
      }

      steps{
        echo "Cleaning up"
        deleteDir()

        unstash "Source"

        echo "Create build folder"
        bat 'mkdir build'


        dir('build') {
          echo "Configuring"
          bat 'cmake ..'

          echo "Building"
          bat 'cmake --build . --config Release'

        }
        stash includes: '**', name: "Windows_Binary_32"
      }
    }

    stage("Windows Building 64 bit") {
      agent{
        label 'Windows'
      }

      steps{
        echo "Cleaning up"
        deleteDir()

        unstash "Source"

        echo "Create build folder"
        bat 'mkdir build'


        dir('build') {
          echo "Configuring"
          bat 'cmake .. -G "Visual Studio 14 2015 Win64"'

          echo "Building"
          bat 'cmake --build . --config Release'

        }
        stash includes: '**', name: "Windows_Binary_64"
      }
    }

    stage("Windows package 32 bit") {
      agent{
        label 'Windows'
      }

      steps{
        deleteDir()
        unstash "Windows_Binary_32"
        dir("build"){
          echo 'Packaging into a zip file'
          bat 'cpack -G ZIP -D CPACK_OUTPUT_FILE_PREFIX=../dist'
        }

        stash includes: 'dist/**', name: "Windows_packaged_32"
      }
    }
    stage("Windows package 64 bit") {
      agent{
        label 'Windows'
      }

      steps{
        deleteDir()
        unstash "Windows_Binary_64"
        dir("build"){
          echo 'Packaging into a zip file'
          bat 'cpack -G ZIP -D CPACK_OUTPUT_FILE_PREFIX=../dist'
        }

        stash includes: 'dist/**', name: "Windows_packaged_64"
      }
    }


    stage("Archiving") {
      agent any

      steps {
        deleteDir()
        unstash "Windows_packaged_64"
        unstash "Windows_packaged_32"
        archiveArtifacts "**"
      }
    }
  }

}
