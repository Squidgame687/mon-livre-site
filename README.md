<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Squid Game: Le Livre</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .container {
            text-align: center;
            max-width: 600px;
            margin: 0 auto;
            background-color: rgb(0, 43, 186);
            padding: 30px;
            box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
        }
        h1 {
            color: #333;
        }
        .btn {
            background-color: #c10087;
            color: white;
            padding: 15px 30px;
            font-size: 18px;
            border: none;
            cursor: pointer;
            border-radius: 5px;
            text-decoration: none;
        }
        .btn:hover {
            background-color: #a9a900;
        }
        form input {
            padding: 10px;
            margin: 10px 0;
            width: 100%;
            border: 1px solid #00d1c6;
            border-radius: 5px;
        }
        .footer {
            text-align: center;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Squid Game: Le Livre</h1>
        <p>Commandez maintenant votre exemplaire du livre Squid Game! Entrez vos informations pour recevoir votre copie.</p>
        
        <form action="order_submission_url" method="POST">
            <input type="text" name="full_name" placeholder="Votre nom complet" required><br>
            <input type="email" name="email" placeholder="Votre email" required><br>
            <input type="text" name="address" placeholder="Adresse de livraison" required><br>
            <button type="submit" class="btn">Commander maintenant</button>
        </form>

        <div class="footer">
            <p>Merci de commander chez nous!</p>
        </div>
    </div>
</body>
</html>from flask import Flask, request, render_template
import smtplib
from email.mime.text import MIMEText

app = Flask(__name__)

def send_confirmation_email(user_email):
    msg = MIMEText('Merci pour votre commande de "Squid Game: Le Livre". Votre commande a bien été reçue.')
    msg['Subject'] = 'Confirmation de commande'
    msg['From'] = 'votre-email@example.com'
    msg['To'] = user_email

    with smtplib.SMTP('smtp.example.com') as server:
        server.login('votre-email@example.com', 'votre-mot-de-passe')
        server.sendmail('votre-email@example.com', user_email, msg.as_string())

@app.route('/', methods=['GET', 'POST'])
def order():
    if request.method == 'POST':
        full_name = request.form['full_name']
        email = request.form['email']
        address = request.form['address']
        
        send_confirmation_email(email)
        
        return render_template('confirmation.html', name=full_name)

    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
