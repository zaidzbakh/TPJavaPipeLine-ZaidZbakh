# TP Java Jenkins Pipeline

## 1. Description du projet
Ce projet illustre la création d'un **pipeline Jenkins** pour un projet Java Maven.  
Le pipeline effectue les étapes suivantes :  
1. Checkout du code depuis GitHub  
2. Compilation et tests Maven  
3. Packaging du projet en jar exécutable  
4. Exécution du jar  

Ce TP permet de mettre en pratique les notions de **DevOps**, **CI/CD**, et **intégration Maven dans Docker via Jenkins**.

---

## 2. Structure du projet

TPJavaPipeLine-ZaidZbakh/
├─ maven/
│ ├─ src/main/java/com/example/Main.java
│ └─ pom.xml
└─ Jenkinsfile

markdown

- `maven/` : Projet Maven avec le fichier Java principal `Main.java`  
- `pom.xml` : Configuration Maven pour compiler et générer un jar exécutable  
- `Jenkinsfile` : Définition du pipeline Jenkins

---

## 3. Contenu du Jenkinsfile

<img width="1891" height="862" alt="i1" src="https://github.com/user-attachments/assets/74700f4d-2eec-4b32-8068-ba0d37b26ffb" />


Le pipeline est défini avec :  

- **Agent Docker** : image contenant Maven et Git  
- **Args** : volume Maven pour réutiliser le cache  
- **Stages** :  
  1. **Checkout** : cloner le repo Git  
  2. **Build** : compiler et packager le projet Maven  
  3. **Execute** : exécuter le jar généré
 
<img width="1818" height="908" alt="i2" src="https://github.com/user-attachments/assets/7b65b7d2-b48f-42c2-b0f4-095a1a34c791" />


Exemple :

```groovy
pipeline {
    agent {
        docker {
            image 'my-maven-git:latest'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                sh "rm -rf *"
                sh "git clone https://github.com/zaidzbakh/TPJavaPipeLine-ZaidZbakh.git"
            }
        }
        stage('Build') {
            steps {
                script {
                    dir('TPJavaPipeLine-ZaidZbakh/maven') {
                        sh 'mvn clean test package'
                        sh "java -jar target/TpJenkins-0.0.1-SNAPSHOT.jar"
                    }
                }
            }
        }
    }
}
