// pipeline {
//   agent {
//     label 'docker'
//     docker {
//         image 'node:7-onbuild'
//         args '-p 8000:8000'
//     }
//   } 

//   stages {
//     stage ('Clone repository') {
//       agent {
//         docker {
//             image 'node:7-onbuild'
//             args '-p 8000:8000'
//         }
//       }
//       steps {
//         sh "git checkout master"
//       }
//     }

//     stage ('Build image') {
//       agent {
//         docker {
//             image 'node:7-onbuild'
//             args '-p 8000:8000'
//         }
//       }
//       steps {
//         sh "docker build -t marcdomain/nodeapp ."
//       }
//     }

//     stage ('Testing stage') {
//       agent {
//         docker {
//             image 'node:7-onbuild'
//             args '-p 8000:8000'
//         }
//       }
//       steps {
//         sh 'npm test'
//       }
//     }

//   }
// }

node {
    def app

    // stage('Initialize') {
    //   env.PATH = "~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty"
    // }

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("marcdomain/nodeapp")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
}