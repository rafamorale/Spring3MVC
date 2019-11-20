def despliegueADes() {  
   echo 'Despliego a desarrollo'
   deploy adapters: [tomcat8(credentialsId: '9069e109-cd12-4eae-a36c-50b270f4eeb5', path: '', url: 'http://localhost:8081')], contextPath: null, war: '**/*.war'
}

def despliegueAPro() { 
   echo 'Despliego a produccion'
   deploy adapters: [tomcat8(credentialsId: '9069e109-cd12-4eae-a36c-50b270f4eeb5', path: '', url: 'http://localhost:8082')], contextPath: null, war: '**/*.war'
}

pipeline {
    agent any
    tools {
        maven 'LocalMaven'
	jdk 'LocalJDK8'
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
			//checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
			checkstyle canComputeNew: false, defaultEncoding: '', failedTotalAll: '10', healthy: '', pattern: '', unHealthy: ''
			pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
			echo 'Finalizado el análisis de calidad de código'
              	}
	      	post { 
        		always { 
				echo 'Me ejecuto siempre'
				echo 'Job 3 paralelo: Muestra por consola el JOB_NAME que tiene y la BUILD'
				echo "El job name es: ${JOB_NAME} Y el numero de build es ${BUILD_ID}"
        		}
			unstable { 
            			echo 'Me ejecuto sólo si es unstable el paso anterior de calidad'
				echo 'Job 4 paralelo: Hace deploy en tomcat de development'
				despliegueADes()
        		}
			success { 
            			echo 'Me ejecuto sólo si hay éxito en el paso anterior de calidad'
				echo 'Job 4 paralelo: Hace deploy en tomcat de development'
				despliegueADes()
				echo 'Job 5 paralelo: Hace deploy en tomcat de producción'
				despliegueAPro()
        		}
    		}
            }
    }
}
