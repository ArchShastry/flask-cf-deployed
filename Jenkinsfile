pipeline {
//None parameter in the agent section means that no global agent will be allocated for the entire Pipeline’s
//execution and that each stage directive must specify its own agent section.
    agent any
    stages {
        stage('Build') {
            steps {
                bat 'python -m py_compile app.py'
                stash(name: 'compiled-results', includes: '*.py*')
            }
        }
        
        
        stage('Publish') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'anagha-creds', usernameVariable: 'AnaghaDAnanth', passwordVariable: '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']]) {                   
                    
                    bat('git push https://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@github.com/AnaghaDAnanth/flask-cf-deployed.git HEAD:refs/heads/main --tags -f --no-verify')
                }
            }
        }

        stage("Py Test") {
            steps{
                
                //bat('PYENV_HOME=$WORKSPACE/.pyenv/virtualenv --no-site-packages $PYENV_HOME source $PYENV_HOME/bin/activate pip install -U pytest pip install -r requirements.txt py.test test_app.py deactivate')
                //bat 'pytest --junitxml results.xml test_app.py'
                bat 'python -m pytest --junitxml results.xml test_routes.py'
                
                //bat 'python -m pip install –upgrade pip'
                //bat 'pip3 install --user -r requirements.txt'
                //bat 'pip3 install -U pytest'
                //bat 'py.test test_app.py'
            }
        }
        
//         stage('Python pytest Tests') {
//             steps{
                
//                 bat 'pip install -r requirements.txt'
//                 bat 'pytest --junit-xml=test_results.xml test || true'
//                 junit keepLongStdio: true, allowEmptyResults: true, testResults: 'test_results.xml'
//             }
//         }
        
        
//         stage("Selenium Test") {
//             steps{
//                   bat 'python -m py_compile Selenium.py'
//             }
            
//              post {
//         always {
        
//             junit 'build/reports/**/*.xml'
//         }
//     }
//         }

        stage('SonarQube analysis') {

        steps {
                script {
                    scannerHome = tool 'SonarqubeScanner-4.7';
                }
                withSonarQubeEnv('Sonarqube') {
                    bat "${scannerHome}/bin/sonar-scanner.bat" 
                }
            }
        }
    }
}

