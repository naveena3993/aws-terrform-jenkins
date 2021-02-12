pipeline {
    agent any
  stages {
        stage('Git checkout') {
      steps {
        git credentialsId: '836f1cd0-78c0-4c51-a21c-d365f759f7b6', url: 'https://github.com/naveena3993/aws-terrform-jenkins.git'
      }
        }
  // Run terraform init
  stage('init') {
    node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        credentialsId: aws-dev-cred,
        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
      ]]) {
        sh ''' terraform init '''
         // bat  "for /D %G in ("*") do cd "%~fG" & terraform init & cd .."

      }
    }
  }

  // Run terraform plan
  stage('plan') {
    node {
      withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        credentialsId: aws-dev-cred,
        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
      ]]) {
	      sh '''
	cd /var/lib/jenkins & terraform plan
	     '''
       // bat "for /D %G in ("*") do cd "%~fG" & terraform plan & cd .."

      }
    }
  }

    // Run terraform apply
    stage('apply') {
      node {
        withCredentials([[
          $class: 'AmazonWebServicesCredentialsBinding',
          credentialsId: aws-dev-cred,
          accessKeyVariable: 'AWS_ACCESS_KEY_ID',
          secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        ]]) {
sh '''
	cd /var/lib/jenkins & terraform apply -auto-approve
	'''
        // bat "for /D %G in ("*") do cd "%~fG" & terraform apply -auto-approve & cd .. "
          
        }
      }

  }
  currentBuild.result = 'SUCCESS'
}
catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException flowError) {
  currentBuild.result = 'ABORTED'
}
catch (err) {
  currentBuild.result = 'FAILURE'
  throw err
}
finally {
  if (currentBuild.result == 'SUCCESS') {
    currentBuild.result = 'SUCCESS'
  }
}
