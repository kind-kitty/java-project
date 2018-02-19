pipeline {
    agent {
        label 'master'
    }
	
    stages {
		stage ( 'Unit Test') {
            steps {
            
                sh 'ant -f test.xml -v'
				junit 'reports/result.xml'
            }
        }
		
        stage ( 'Build') {
            steps {
                
                sh 'ant -f build.xml -v'
				
            }
			post {
				success {
					archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
				}

			}
        }
		
		stage ( 'Deploy') {
            steps {
				sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"
                sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
            }
        }
		
		stage ( 'Functional testing'){
			steps {
				sh "wget http://srikavyakola3.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
				sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 4 5"
			}
		}
		stage ( 'Promote to green') {
			when {
				branch 'development'
			}
			steps {
				sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
			}
		
		}
		stage ('Promoting master branch to developement') {
			when {
				branch 'master'
			}
			steps {
				echo "stashing any local changes"
				sh 'git stash'
				echo "checking out master branch"
				sh 'git checkout master'
				echo "checking out developement branch"
				sh 'git checkout development'
				echo "merging master into development"
				sh 'git merge master'
				echo "pushing to origin development"
				sh 'git push origin development'
			}
		}
	}
}






















