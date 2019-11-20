pipeline {
    agent any
    tools {
        maven 'LocalMaven'
    }
    stages {	
	    stage('CleanPackage') {
	    	steps {
                	echo 'Job 1 serie'
			echo 'Iniciando clean y package'
			bat 'MiMaven.bat'
			echo 'Finalizados clean y package'
              	}
            }
	    stage('Calidad') {
	      	steps {
                	echo 'Job 2 serie'
			echo 'Iniciando análisis de calidad de código'
			bat 'MiCalidad.bat'
			echo 'Finalizado el análisis de calidad de código'
              	}
	      	post { 
        		always { 
				echo 'Me ejecuto siempre'
				echo 'Job 3 paralelo: Muestra por consola el JOB_NAME que tiene y la build'
        		}
			unstable { 
            			echo 'Me ejecuto sólo si es unstable el paso anterior de calidad'
				echo 'Job 4 paralelo: Hace deploy en tomcat de development'
        		}
			success { 
            			echo 'Me ejecuto sólo si hay éxito en el paso anterior de calidad'
				echo 'Job 4 paralelo: Hace deploy en tomcat de development'
				echo 'Job 5 paralelo: Hace deploy en tomcat de producción'
        		}
    		}
            }
    }
}
