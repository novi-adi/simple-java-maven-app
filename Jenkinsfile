node {
    docker.image('maven:3.9.0-eclipse-temurin-11').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {
            sh 'mvn -B -DskipTests clean package'
        }

        stage('Test') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }

        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input(
                        message: 'Lanjutkan ke tahap Deploy?',
                        parameters: [
                            [$class: 'ChoiceParameterDefinition', choices: 'PROCEED\nABORT', description: 'Pilih PROCEED untuk melanjutkan atau ABORT untuk menghentikan eksekusi pipeline.']
                        ]
                    )
                    if (userInput == 'PROCEED') {
                        echo 'User chose to proceed to Deploy stage.'
                    } else {
                        error('Pipeline execution aborted by the user.')
                    }
                }
            }
        }

        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'

            // Pause the pipeline execution for 1 minute (60 seconds)
            sleep time: 60, unit: 'SECONDS'
        }
    }
}