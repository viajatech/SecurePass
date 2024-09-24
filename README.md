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
 Las contraseñas se cifran utilizando el algoritmo AES-256 en modo CBC (Cipher Block Chaining).
Derivación de Clave:
PBKDF2HMAC: Se utiliza para derivar una clave de cifrado a partir de la contraseña maestra proporcionada por el usuario.
Sal y Iteraciones: Se emplea una sal aleatoria y 200,000 iteraciones para incrementar la seguridad contra ataques de fuerza bruta.
Proceso de Cifrado:
Generación de la Clave: A partir de la contraseña maestra y una sal almacenada en lock_info.json.
Cifrado de la Contraseña: La contraseña ingresada se cifra usando AES-CBC con un IV (Vector de Inicialización) aleatorio.
Almacenamiento: La contraseña cifrada se codifica en Base64 y se guarda en el campo password de la tabla passwords.
2. Archivo de Información de Bloqueo (lock_info.json)
Ubicación:

Se encuentra en el mismo directorio que el script de Python.
Contenido:

salt: La sal utilizada para derivar la clave de cifrado a partir de la contraseña maestra.
password_hash: El hash SHA-256 de la contraseña maestra para verificar su validez durante el inicio de sesión.
Protección del Archivo:

Ocultación del Archivo:
Windows: Se utiliza el comando attrib +h para ocultar el archivo.
Linux/macOS: Se renombra agregando un punto (.) al inicio del nombre del archivo, lo que lo hace oculto en la mayoría de los exploradores de archivos.
Importancia de la Sal y el Hash:
La sal es esencial para derivar la clave de cifrado de manera segura.
El hash de la contraseña maestra se utiliza para autenticar al usuario sin almacenar la contraseña en texto plano.
