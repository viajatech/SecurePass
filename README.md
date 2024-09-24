El script "SecurePass by Viaja Tech" implementa un sistema robusto para almacenar y gestionar contraseñas de manera segura mediante una base de datos cifrada y una contraseña maestra protegida. Las contraseñas se almacenan en el archivo securepass.db en la tabla passwords, mientras que la información crucial para la verificación de la contraseña maestra se guarda en lock_info.json. Ambos archivos están protegidos mediante técnicas de ocultación específicas del sistema operativo para impedir accesos no autorizados.
--------
![](https://github.com/viajatech/SecurePass/blob/main/SecurePass%20Foto%20GUI%20.png)
--------
![](https://github.com/viajatech/SecurePass/blob/main/GUI%20SECURE%20PASS%20QR%20GENERADOR.png)
--------
![](https://github.com/viajatech/SecurePass/blob/main/verificar%20QR%20.png)
--------
![](https://github.com/viajatech/SecurePass/blob/main/agregar%20entrada.png)
--------
![](https://github.com/viajatech/SecurePass/blob/main/Generar%20Contrase%C3%B1a%20GUI.png)
--------
Seguridad de la Contraseña Maestra
Hashing Seguro:
La contraseña maestra no se almacena directamente. En su lugar, se guarda un hash SHA-256 de la contraseña, lo que añade una capa de seguridad.
Verificación de Contraseña:
Al iniciar sesión, la contraseña ingresada se hashée y se compara de manera segura (usando hmac.compare_digest) con el hash almacenado en lock_info.json.
Autenticación Multifactorial (MFA)
Configuración de 2FA:
Si habilitas 2FA, se almacena un secreto TOTP (Time-based One-Time Password) en la tabla settings de la base de datos.
Proceso de Autenticación:
Al iniciar sesión, después de ingresar la contraseña maestra, se solicita un código OTP generado por una aplicación de autenticación (como Google Authenticator).
Protección Contra Ataques de Fuerza Bruta
Límites de Intentos:
El script limita el número de intentos de inicio de sesión fallidos a 5.
Bloqueo Temporal:
Tras alcanzar el máximo de intentos fallidos, el acceso se bloquea durante 5 minutos para prevenir ataques de fuerza bruta.
