pipeline{

   agent any

   parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
   }


    stages{

        stage('Start Grid'){
            steps{
                bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }

        stage('Run Test'){
            steps{
                bat "docker-compose -f test-suites.yaml up --pull=always"
            }
        }

    }

    post {
        always {
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/saucelabs/emailable-report.html', followSymlinks: false
        }
    }

}