[index (1).html](https://github.com/user-attachments/files/23234573/index.1.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inscrição Treinamentos NR35 e NR18 - DHL Supply Chain</title>
    <style>
        :root {
            --dhl-yellow: #FFCC00;
            --dhl-red: #D40511;
            --dhl-dark-red: #B00000;
            --dhl-grey: #444444;
            --dhl-light-grey: #f4f4f4;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: var(--dhl-light-grey);
            color: var(--dhl-grey);
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background-color: #ffffff;
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            border-bottom: 3px solid var(--dhl-yellow);
            padding-bottom: 15px;
        }

        .logo {
            color: var(--dhl-red);
            font-size: 36px;
            font-weight: bold;
            margin-bottom: 5px;
            letter-spacing: 2px;
            /* Simulação do logo DHL */
        }

        .logo-subtext {
            color: var(--dhl-yellow);
            font-size: 18px;
            font-weight: bold;
            display: block;
            margin-top: -10px;
        }
        
        .header h1 {
            color: var(--dhl-grey);
            font-size: 20px;
            margin-top: 10px;
        }

        .header h2 {
            font-size: 14px;
            font-weight: normal;
            color: #666;
            margin-top: 5px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: var(--dhl-grey);
        }

        input[type="text"],
        select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input[type="text"]:focus,
        select:focus {
            border-color: var(--dhl-yellow);
            outline: none;
            box-shadow: 0 0 5px rgba(255, 204, 0, 0.5);
        }

        .button-submit {
            background-color: var(--dhl-red);
            color: white;
            padding: 15px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
            font-size: 18px;
            font-weight: bold;
            transition: background-color 0.3s, box-shadow 0.3s;
        }

        .button-submit:hover {
            background-color: var(--dhl-dark-red);
            box-shadow: 0 4px 8px rgba(212, 5, 17, 0.4);
        }

        .success-message {
            display: none;
            text-align: center;
            padding: 20px;
            background-color: #e6ffe6;
            border: 1px solid #00cc00;
            color: #008000;
            border-radius: 4px;
            margin-top: 20px;
            font-weight: bold;
        }

        .error-message {
            display: none;
            text-align: center;
            padding: 10px;
            background-color: #ffe6e6;
            border: 1px solid #cc0000;
            color: #cc0000;
            border-radius: 4px;
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="logo">DHL</div>
            <span class="logo-subtext">Supply Chain</span>
            <h1>Inscrição Para Treinamento</h1>
            <h2>NR 35 (Trabalho em Altura) e NR 18 (Operação de Plataformas Elevatórias)</h2>
        </div>

        <div id="error-message" class="error-message"></div>

        <form id="registration-form">
            <div class="form-group">
                <label for="nome">Nome Completo:</label>
                <input type="text" id="nome" name="nome" required placeholder="Seu nome completo">
            </div>

            <div class="form-group">
                <label for="matricula">Matrícula (Crachá):</label>
                <input type="text" id="matricula" name="matricula" required pattern="[0-9]{4,}" title="A matrícula deve conter apenas números e ter no mínimo 4 dígitos." placeholder="Ex: 12345">
            </div>

            <div class="form-group">
                <label for="turno">Turno de Trabalho:</label>
                <select id="turno" name="turno" required>
                    <option value="">Selecione seu turno</option>
                    <option value="Manhã">Manhã (Ex: 06:00 - 14:00)</option>
                    <option value="Tarde">Tarde (Ex: 14:00 - 22:00)</option>
                    <option value="Noite">Noite (Ex: 22:00 - 06:00)</option>
                    <option value="Administrativo">Administrativo</option>
                    <option value="Outro">Outro</option>
                </select>
            </div>

            <button type="submit" class="button-submit">Confirmar Inscrição</button>
        </form>

        <div id="success-message" class="success-message">
            Inscrição realizada com sucesso! Seus dados foram enviados para a planilha de controle.
        </div>
    </div>

    <script>
        // *** IMPORTANTE: SUBSTITUA ESTA URL PELA URL DO SEU GOOGLE APPS SCRIPT DEPLOYADO ***
        const WEB_APP_URL = "https://script.google.com/macros/s/AKfycbze47kd37dj2Ujc6rtt7pBOuRs6ZMkhyD60b8hZBtYWwc0nhcW21HoELNXwcIq5FSUHgA/exec"; 

        document.getElementById('registration-form').addEventListener('submit', async function(event) {
            event.preventDefault();
            
            const form = event.target;
            const submitButton = form.querySelector('.button-submit');
            const errorMessage = document.getElementById('error-message');
            const successMessage = document.getElementById('success-message');

            errorMessage.style.display = 'none';
            successMessage.style.display = 'none';

            // Validação básica
            if (!form.checkValidity()) {
                errorMessage.textContent = 'Por favor, preencha todos os campos corretamente.';
                errorMessage.style.display = 'block';
                return;
            }

            if (WEB_APP_URL === "SUA_URL_DO_APPS_SCRIPT_AQUI") {
                 errorMessage.textContent = 'ERRO: A URL do Google Apps Script não foi configurada. Por favor, siga as instruções para obter a URL e substitua a variável WEB_APP_URL no código.';
                 errorMessage.style.display = 'block';
                 return;
            }

            // Coleta dos dados
            const formData = new FormData(form);
            
            // Desabilita o botão para evitar múltiplos envios
            submitButton.disabled = true;
            submitButton.textContent = 'Enviando...';

            try {
                const response = await fetch(WEB_APP_URL, {
                    method: 'POST',
                    body: formData // Envia os dados como form-data
                });

                const result = await response.json();

                if (result.result === 'success') {
                    form.reset();
                    form.style.display = 'none';
                    successMessage.style.display = 'block';
                } else {
                    errorMessage.textContent = 'Erro ao enviar os dados: ' + result.message;
                    errorMessage.style.display = 'block';
                }

            } catch (error) {
                errorMessage.textContent = 'Erro de conexão ou servidor: ' + error.message;
                errorMessage.style.display = 'block';
            } finally {
                submitButton.disabled = false;
                submitButton.textContent = 'Confirmar Inscrição';
            }
        });
    </script>
</body>
</html>
