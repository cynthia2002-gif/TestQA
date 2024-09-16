# TestQA*
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time

# Remplacez le chemin par le chemin de votre WebDriver (par exemple, chromedriver)
driver_path = "/path/to/chromedriver"
driver = webdriver.Chrome(driver_path)

# URL de la page de création de compte
url = "https://example.com/create-account"

# Ouvrir la page de création de compte
driver.get(url)

def test_creation_account():
    # Vérifier la présence de tous les champs obligatoires
    try:
        email_field = driver.find_element(By.ID, "email")
        password_field = driver.find_element(By.ID, "password")
        submit_button = driver.find_element(By.ID, "submit")
        print("Tous les champs obligatoires sont présents.")
    except Exception as e:
        print(f"Erreur : Un ou plusieurs champs obligatoires sont manquants - {str(e)}")

    # Test de validation de l'email
    email_field.clear()
    email_field.send_keys("invalid-email-format")
    submit_button.click()
    time.sleep(2)  # Attendre le traitement
    try:
        email_error = driver.find_element(By.ID, "email-error")
        print("Validation de l'email fonctionne correctement : erreur affichée pour un format invalide.")
    except Exception:
        print("Erreur : La validation de l'email ne fonctionne pas correctement.")

    # Test de validation du mot de passe (par exemple, trop faible)
    email_field.clear()
    email_field.send_keys("user@example.com")
    password_field.clear()
    password_field.send_keys("12345")  # Mot de passe faible
    submit_button.click()
    time.sleep(2)  # Attendre le traitement
    try:
        password_error = driver.find_element(By.ID, "password-error")
        print("Validation du mot de passe fonctionne correctement : erreur affichée pour un mot de passe faible.")
    except Exception:
        print("Erreur : La validation du mot de passe ne fonctionne pas correctement.")

    # Test de création de compte avec succès
    email_field.clear()
    password_field.clear()
    email_field.send_keys("user@example.com")
    password_field.send_keys("StrongPassword123!")
    submit_button.click()
    time.sleep(3)  # Attendre le traitement et la redirection
    try:
        success_message = driver.find_element(By.ID, "success-message")
        print("Création de compte réussie.")
    except Exception:
        print("Erreur : La création de compte a échoué.")

    # Test du système de vérification de l'email
    # Cette partie est généralement complexe car elle implique une action externe (réception d'un email)
    # Ici, nous faisons une hypothèse que la page affiche un message de vérification d'email.
    try:
        email_verification_message = driver.find_element(By.ID, "email-verification-message")
        print("Système de vérification d'email fonctionne correctement.")
    except Exception:
        print("Erreur : Système de vérification d'email ne fonctionne pas correctement.")
        
    # Ajoutez des tests supplémentaires si nécessaire

# Appel de la fonction de test
test_creation_account()

# Fermer le navigateur
driver.quit()
