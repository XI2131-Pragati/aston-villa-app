pipeline {
    agent any
    stages{
        stage('Build container image') {
        //Build the container image with a tag (qualys:sample in this case)
            steps {
                dir("dockerbuild") {
                sh "docker build -t qualys:sample . > docker_output"
            }
            }
        }

        stage('Get Image id') {
        //Use the same repo:tag (qualys:sample in this case) combination with the grep command to get the same image id and save the image id in an environment variable
            steps {
            script {
                def IMAGE_ID = sh(script: "docker images | grep -E '^qualys.*sample' | head -1 | awk '{print \$3}'", returnStdout: true).trim()
                env.IMAGE_ID = IMAGE_ID
        }
        }
        }

        stage('Get Image Vulns - Qualys Plugin') {
        //Use the same environment variable(env.IMAGE_ID) as an input to Qualys Plugin's step
            steps {
                getImageVulnsFromQualys useGlobalConfig:true,
                imageIds: env.IMAGE_ID
        }
        }

}}
