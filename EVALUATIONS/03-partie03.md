# Évaluation : Création d’un CV interactif en utilisant l’IA

# Évaluation : Création et Déploiement d’un CV interactif sur AWS Amplify

**Objectif** : Créer un CV interactif en format HTML, stylisé avec Bootstrap et FontAwesome, puis déployer ce CV sur AWS Amplify. Vous n'aurez pas besoin d'écrire du code HTML, seulement de fournir vos informations personnelles et professionnelles, puis d'utiliser un générateur d’IA et AWS pour déployer votre CV en ligne.

---
## Étapes à suivre :
---

---
# Étape 1 : Préparer vos informations personnelles
---


1. **Préparez les informations suivantes** pour alimenter votre CV :
   - **Nom complet**
   - **Poste actuel** ou celui que vous visez
   - **Courte description personnelle** (2 à 3 phrases résumant votre parcours ou vos objectifs)
   - **Liste de 3 à 5 compétences principales** (ex. : Python, JavaScript, Gestion de projet, etc.)
   - **Expériences professionnelles** (nom de l'entreprise, poste occupé, période, et brève description)
   - **Photo de profil** (optionnelle, sinon utilisez un placeholder)

---
# Étape 2 : Utiliser l'IA pour générer votre CV en HTML
---


2. **Utilisez un générateur d'IA comme ChatGPT** pour créer votre CV automatiquement.

   - Accédez à un générateur d'IA tel que **ChatGPT**.
   - Utilisez le prompt suivant pour générer votre CV :

   ```
   Génère un CV en format HTML avec le modèle suivant. Utilise les informations suivantes pour remplir les champs :
   - Nom : [Votre nom]
   - Poste : [Votre poste]
   - Description : [Votre description personnelle]
   - Compétences : [Votre liste de compétences]
   - Expériences professionnelles : [Votre liste d'expériences]
   - Image : [Lien vers votre photo ou utiliser un placeholder]
   
   Modèle HTML :
   
   <!DOCTYPE html>
   <html lang="fr">
   
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Notre Équipe</title>
       <link rel="stylesheet" target=_blank href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
       <link rel="stylesheet" target=_blank href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
   </head>
   
   <body>
   
       <!-- Jumbotron pour l'en-tête -->
       <div class="jumbotron text-center">
           <h1>Notre Équipe</h1>
           <p>Rencontrez les esprits brillants derrière notre succès!</p>
       </div>
   
       <!-- Grille pour les profils de l'équipe -->
       <div class="container">
   
           <!-- [Nom] -->
           <div class="row mb-4">
               <div class="col-md-4">
                   <img src="[Lien de la photo]" alt="[Nom]" class="img-fluid rounded-circle">
               </div>
               <div class="col-md-4">
                   <h3>[Nom] <i class="fas fa-user-tie"></i></h3>
                   <p>[Poste]</p>
                   <p>[Description personnelle]</p>
               </div>
               <div class="col-md-4">
                   <h4>Compétences</h4>
                   <span class="badge badge-primary"><i class="fab fa-python"></i> Python</span>
                   <span class="badge badge-success"><i class="fab fa-js-square"></i> JavaScript</span>
               </div>
           </div>
   
           <!-- Expériences professionnelles -->
           <div class="container">
               <h2>Expériences professionnelles</h2>
               <div class="row mb-4">
                   <div class="col-md-12">
                       <h4>[Nom de l'entreprise]</h4>
                       <p><strong>[Poste occupé]</strong> - [Période]</p>
                       <p>[Brève description de l'expérience]</p>
                   </div>
               </div>
           </div>
   
       </div>
   
       <!-- Bootstrap JS -->
       <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
       <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
       <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
   
   </body>
   
   </html>
   ```

   - L'IA générera pour vous un fichier HTML complet avec vos informations.

3. **Téléchargez le fichier généré** et nommez-le `index.html`.

4. **Créez un fichier zip** contenant ce fichier `index.html`. 
   - Faites un clic droit sur `index.html` et sélectionnez "Compresser en fichier zip".

---
# Étape 3 : Déployer votre CV sur AWS Amplify
---

1. **Accédez à AWS Amplify** :
   - Connectez-vous à votre compte AWS.
   - Accédez à la section **AWS Amplify** via la console AWS.

2. **Créer une nouvelle application hébergée** :
   - Cliquez sur **Get Started** dans la section "Host your web app".
   - Choisissez l'option **Déploiement manuel** ("Deploy without Git").
   - Sélectionnez **Déployer via ZIP**.

3. **Téléversez votre fichier zip** :
   - Cliquez sur **Télécharger** et sélectionnez le fichier zip que vous avez créé contenant votre fichier `index.html`.

4. **Déployez votre CV** :
   - Une fois le fichier téléchargé, AWS Amplify générera une URL que vous pourrez partager.

5. **Tester l'URL** :
   - Ouvrez l'URL générée par AWS Amplify pour voir votre CV en ligne.

---
### Étape 4 : Soumettre votre travail
---

1. **Envoyez l'URL générée** par AWS Amplify une fois votre CV déployé.

---

# Critères de réussite :

1. Le fichier HTML est correctement généré et contient toutes vos informations personnelles et professionnelles.
2. Le fichier a été correctement zippé et téléversé sur AWS Amplify.
3. Votre CV est déployé et accessible via une URL fonctionnelle.
4. L'URL du CV déployé est fournie lors de la soumission.













-------------
# Annexe
-------------

``` html
<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Notre Équipe</title>
    <link rel="stylesheet" target=_blank href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <link rel="stylesheet" target=_blank href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
</head>

<body>

    <!-- Jumbotron pour l'en-tête -->
    <div class="jumbotron text-center">
        <h1>Notre Équipe</h1>
        <p>Rencontrez les esprits brillants derrière notre succès!</p>
    </div>

    <!-- Grille pour les profils de l'équipe -->
    <div class="container">

        <!-- Jean Dupont -->
        <div class="row mb-4">
            <div class="col-md-4">
                <img src="https://via.placeholder.com/150?text=Jean+Dupont" alt="Jean Dupont" class="img-fluid rounded-circle">
            </div>
            <div class="col-md-4">
                <h3>Jean Dupont <i class="fas fa-user-tie"></i></h3>
                <p>Ingénieur en Chef</p>
                <p>Passionné par les technologies et le design, Jean est le moteur technique de notre équipe.</p>
            </div>
            <div class="col-md-4">
                <h4>Compétences</h4>
                <span class="badge badge-primary"><i class="fab fa-python"></i> Python</span>
                <span class="badge badge-success"><i class="fab fa-django"></i> Django</span>
                <span class="badge badge-warning"><i class="fab fa-js-square"></i> JavaScript</span>
            </div>
        </div>

        <!-- Haythem Rehouma -->
        <div class="row mb-4">
            <div class="col-md-4">
                <img src="https://via.placeholder.com/150?text=Haythem+Rehouma" alt="Haythem Rehouma" class="img-fluid rounded-circle">
            </div>
            <div class="col-md-4">
                <h3>Haythem Rehouma <i class="fas fa-cloud"></i></h3>
                <p>Architecte AWS</p>
                <p>Expert en infrastructure cloud, Haythem structure et optimise nos solutions sur AWS.</p>
            </div>
            <div class="col-md-4">
                <h4>Compétences</h4>
                <span class="badge badge-info"><i class="fab fa-aws"></i> AWS</span>
                <span class="badge badge-secondary"><i class="fas fa-server"></i> Infrastructures</span>
            </div>
        </div>

        <!-- Elie Simard -->
        <div class="row mb-4">
            <div class="col-md-4">
                <img src="https://via.placeholder.com/150?text=Elie+Simard" alt="Elie Simard" class="img-fluid rounded-circle">
            </div>
            <div class="col-md-4">
                <h3>Elie Simard <i class="fas fa-code"></i></h3>
                <p>Développeur Backend</p>
                <p>Elie est la colonne vertébrale de nos applications, assurant performance et fiabilité du côté serveur.</p>
            </div>
            <div class="col-md-4">
                <h4>Compétences</h4>
                <span class="badge badge-success"><i class="fas fa-database"></i> Bases de données</span>
                <span class="badge badge-warning"><i class="fab fa-php"></i> PHP</span>
            </div>
        </div>

        <!-- Yan Han -->
        <div class="row mb-4">
            <div class="col-md-4">
                <img src="https://via.placeholder.com/150?text=Yan+Han" alt="Yan Han" class="img-fluid rounded-circle">
            </div>
            <div class="col-md-4">
                <h3>Yan Han <i class="fas fa-laptop-code"></i></h3>
                <p>Développeuse Full Stack</p>
                <p>Avec une expertise à la fois frontend et backend, Yan est un atout polyvalent pour notre équipe.</p>
            </div>
            <div class="col-md-4">
                <h4>Compétences</h4>
                <span class="badge badge-danger"><i class="fab fa-node-js"></i> Node.js</span>
                <span class="badge badge-primary"><i class="fab fa-react"></i> React</span>
            </div>
        </div>

        <!-- Cesar -->
        <div class="row mb-4">
            <div class="col-md-4">
                <img src="https://via.placeholder.com/150?text=Cesar" alt="Cesar" class="img-fluid rounded-circle">
            </div>
            <div class="col-md-4">
                <h3>Cesar <i class="fas fa-briefcase"></i></h3>
                <p>Directeur de l'agence</p>
                <p>Le visionnaire derrière notre succès, Cesar dirige l'équipe avec passion et détermination.</p>
            </div>
            <div class="col-md-4">
                <h4>Compétences</h4>
                <span class="badge badge-info"><i class="fas fa-users"></i> Gestion d'équipe</span>
                <span class="badge badge-success"><i class="fas fa-chart-line"></i> Stratégie</span>
            </div>
        </div>
    </div>

    <!-- Les autres sections du code restent inchangées... -->
<!-- Carousel pour les témoignages -->
<div class="container mt-5">
    <h2 class="text-center mb-4">Ce que nos clients disent</h2>
    <div id="testimonialsCarousel" class="carousel slide" data-ride="carousel">
        <ol class="carousel-indicators">
            <li data-target="#testimonialsCarousel" data-slide-to="0" class="active"></li>
            <li data-target="#testimonialsCarousel" data-slide-to="1"></li>
            <li data-target="#testimonialsCarousel" data-slide-to="2"></li>
        </ol>
        <div class="carousel-inner">
            <!-- Témoignage 1 -->
            <div class="carousel-item active">
                <img src="https://via.placeholder.com/800x300?text=Client+A" alt="Client A" class="d-block w-100">
                <div class="carousel-caption">
                    <p>"Ils ont fait un travail exceptionnel sur notre projet!"</p>
                    <h5>- Client A</h5>
                </div>
            </div>
            <!-- Témoignage 2 -->
            <div class="carousel-item">
                <img src="https://via.placeholder.com/800x300?text=Client+B" alt="Client B" class="d-block w-100">
                <div class="carousel-caption">
                    <p>"Service impeccable et équipe très réactive."</p>
                    <h5>- Client B</h5>
                </div>
            </div>
            <!-- Témoignage 3 -->
            <div class="carousel-item">
                <img src="https://via.placeholder.com/800x300?text=Client+C" alt="Client C" class="d-block w-100">
                <div class="carousel-caption">
                    <p>"Une expérience utilisateur comme je n'en ai jamais eu. Bravo!"</p>
                    <h5>- Client C</h5>
                </div>
            </div>
        </div>
        <!-- Contrôles du carousel -->
        <a class="carousel-control-prev" target=_blank href="#testimonialsCarousel" role="button" data-slide="prev">
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
            <span class="sr-only">Précédent</span>
        </a>
        <a class="carousel-control-next" target=_blank href="#testimonialsCarousel" role="button" data-slide="next">
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
            <span class="sr-only">Suivant</span>
        </a>
    </div>
</div>


 <!-- Bootstrap JS (optionnel pour la fonctionnalité du carousel) -->
 <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
 <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
 <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>


</body>

</html>

``` 

