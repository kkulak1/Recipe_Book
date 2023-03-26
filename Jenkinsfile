pipeline {
  agent any

  tools {
      jdk 'jdk-17'
      maven "M3"
      git "Default"
  }

  environment {
    gitUrl = "${env.GITHUB_SERVER_URL}/${env.GITHUB_REPOSITORY}.git"
    commitSha = "${env.GITHUB_SHA}"
    githubToken = "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"
  }

  stages {
    stage("verifying") {
        steps {
            sh "mvn verify"
        }
    }
  }

  post {
    success  {
      script {
        def gitHub = github()
        gitHub.setCommitStatus(
          context: "Jenkins",
          state: "SUCCESS",
          targetUrl: "${env.BUILD_URL}",
          description: "Build Successful",
          sha1: commitSha,
          repoOwner: env.GITHUB_REPOSITORY_OWNER,
          repository: env.GITHUB_REPOSITORY,
          oauthToken: githubToken
        )
      }
    }

    failure  {
      script {
        def gitHub = github()
        gitHub.setCommitStatus(
          context: "Jenkins",
          state: "FAILURE",
          targetUrl: "${env.BUILD_URL}",
          description: "Build Failed",
          sha1: commitSha,
          repoOwner: env.GITHUB_REPOSITORY_OWNER,
          repository: env.GITHUB_REPOSITORY,
          oauthToken: githubToken
        )
      }
    }
  }
}
