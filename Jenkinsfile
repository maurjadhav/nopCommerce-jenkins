pipeline {
    agent {
        label 'dotnet'
    }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('scm') {
            steps {
                git url: 'https://github.com/maurjadhav/nopCommerce-jenkins.git',
                    branch: 'develop'
            }
        }
        stage('build') {
            steps {
                mail bcc: '', body: 'Build started', cc: '', from: '', replyTo: '',
                    subject: "Build started for ${JOB_BASE_NAME} with Build Id ${BUILD_ID}", to: 'all@learnigthoughts.io'

                sh 'dotnet publish -o published/ -c Release src/Presentation/Nop.Web/Nop.Web.csproj'
//                dotnetPublish configuration: 'Release',
//                    outputDirectory: 'published',
//                    project: 'src/Presentation/Nop.Web/Nop.Web.csproj'
            }
            post {
                failure {
                    mail bcc: 'all@learningthoughts.io',
                        from: 'jenkins@learningthouths.io',
                        to: "dev@learningthoughs.io",
                        subject: "Build of ${JOB_BASE_NAME} with Build Id ${BUILD_ID} is failed",
                        body: "Refer to ${RUN_DISPLAY_URL} for more info"
                }
                success {
                    archiveArtifacts artifacts: 'published/**',
                      fingerprint: true

                    mail bcc: 'all@learningthoughts.io',
                        from: 'jenkins@learningthouths.io',
                        to: "dev@learningthoughs.io",
                        subject: "Build of ${JOB_BASE_NAME} with Build Id ${BUILD_ID} is success",
                        body: "Refer to ${RUN_DISPLAY_URL} for more info"
                }
            }
        }

    }
}