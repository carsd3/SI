
<div style="page-break-after: always;"></div>
<div style="page-break-after: always;"></div>

 
# **Introducción**

En esta práctica se va a profundizar en los conceptos de la Seguridad en la Información utilizando la herramienta OpenSSL. En la primera parte de la misma se usará el comando **openssl** para realizar acciones de cifrado simétrico, resúmenes, claves asimétricas y firmas y cifrados asimétricos. En la segunda parte de la práctica se practicará el envío, recepción y **decodificación manual** de mensajes con el estándar **S/MIME**. Finalmente en la tercera y última parte se creará un certificado **autofirmado** de servidor Web utilizando las herramientas del comando **openssl**.

# Parte 1

En esta primera parte se utilizarán los distintos comandos del openssl para el **cifrado y resumen de ficheros y la generación de claves**.

## Parte 1.1 Generación y comprobación de Resúmenes
Una vez entendido el empleo del comando openssl-dgst y sus opciones básicas:
- Crear un archivo de texto legible de entre 150 y 200 caracteres
- Aplicar un mínimo de CINCO algoritmos de resumen (SHA-1 y SHA-2 obligatorios) sobre
ese archivo de texto y comprobar cómo varían los resúmenes obtenidos ante mínimas
modificaciones del fichero (cambiando un solo carácter, por ejemplo).

El comando dgst convierte cualquier entrada en una ristra de bits de un tamaño fijo que es lo que se denomina como resumen o valor hash. Aplicaremo dicho comando en un fichero de texto plano para comprobar cómo varían los resúmenes obtenidos ante la mínima modificación del fichero. Para ello hemos escogido como contenido del fichero los primeros versos de la Ilíada:

```
Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles.
```

A dicho fichero de texto le aplicaremos los siguientes algoritmos de resumen:

- **MD5**<br>
  **Comando**:
  ```
  openssl dgst -md5 ili.txt
  ```
  **Resumen**:
  ```
  MD5(ili.txt)=7ea2260d825ada8937c3ef44b7a5502f
  ```
  <br>
- **SHA-1** <br>
  **Comando**:
  ```
  openssl dgst -sha1 ili.txt
  ```
  **Resumen**:
  ```
  SHA1(ili.txt)=e3d49e5247c3cc7c6d86bee070ee2b6ab11d81e6
  ```
  <br>
- **SHA-2 (224 bits)** <br>
  **Comando**:
  ```
  openssl dgst -sha224 ili.txt
  ```
  **Resumen**:
  ```
  SHA224(ili.txt)=07bf5bf94c5caa8ef57ba1e5c16236e0cfdf15e0f6eb4cbe16efdce2
  ```
  <br>
- **SHA-2 (512 bits)** <br>
  **Comando**:
  ```
  openssl dgst -sha512 ili.txt
  ```
  **Resumen**:
  ```
  SHA512(ili.txt)=92da70753ef385556e34fb2d4082eb8f766e4edf6f09af5428200ef3f80f5f6fc1adeb0b6edd9ccb59a7d728738ca66dbe94243d505c0dfbb9f5673355834c89
  ```
  <br>  
- **SHA-3 (224 bits)** <br>
  **Comando**:
  ```
  openssl dgst -sha3-224 ili.txt
  ```
  **Resumen**:
  ```
  SHA3-224(ili.txt)=b295d604bdcd8ea69205fa08af11a163123991d77dab1dc4f6db0893
  ```
  <br>  

A continuación eliminaremos el punto final del fichero para comprobar cómo cambian los resúmenes:

```
Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles
```

Le aplicamos al fichero modificado los distintos algoritmos para comprobar cómo cambian los resúmenes.

- **MD5**<br>
  **Comando**:
  ```
  openssl dgst -md5 ili.txt
  ```
  **Resumen**:
  ```
  MD5(ili.txt)=826b8e25573a041809c3822d87f87e34
  ```
  <br>
- **SHA-1** <br>
  **Comando**:
  ```
  openssl dgst -sha1 ili.txt
  ```
  **Resumen**:
  ```
  SHA1(ili.txt)=dc28efe25a7ae4230c993641daff9ef4ae0605c0
  ```
  <br>
- **SHA-2 (224 bits)** <br>
  **Comando**:
  ```
  openssl dgst -sha224 ili.txt
  ```
  **Resumen**:
  ```
  SHA224(ili.txt)=c1ff4f1bdf1f1e599fa9112b662550681caf4e4bc3343fbf4484d2e1
  ```
  <br>
- **SHA-2 (512 bits)** <br>
  **Comando**:
  ```
  openssl dgst -sha512 ili.txt
  ```
  **Resumen**:
  ```
  SHA512(ili.txt)=98c15d5cdb69190216d2b1b6246fa1b66a47f62858d2b84cd848746e616b377bd8700eb75f27e87e3967557f08b5dd2560f824a720dd928f609f7decc7100d88
  ```
  <br>  
- **SHA-3 (224 bits)** <br>
  **Comando**:
  ```
  openssl dgst -sha3-224 ili.txt
  ```
  **Resumen**:
  ```
  SHA3-224(ili.txt)=48982675b177a9ca01174ea71240903a980c492206673ae58b836d60
  ```
  <br>  

Como podemos comprobar, al modificar alguno de los caracteres de los ficheros los resúmenes resultantes son completamente diferentes entre cada uno de los resultados de los distintos algoritmos. Se puede ver más claramente en las siguientes imágenes:

![Resúmenes sin modificar](Parte%201/1.1%20parte%201.png)
*Resúmenes sin modificar*
![Resúmenes modificados](Parte%201/1.1%20parte%202.png)
*Resúmenes modificados*

## 1.2 Cifrado Simétrico de documentos
Una vez entendido el empleo del comando openssl-enc para cifrar y descifrar, con diferentes
algoritmos y modos de operación, el empleo de ficheros binarios y BASE64, la obtención de claves
de contraseñas detallada en el estándar PKCS #5 (PBKDF1 y PBKDF2) y su aplicación a las claves
de cifrado simétrico, vectores de inicialización y sal (derivación de claves e “iv”) a partir de
contraseñas:
- Crear un archivo de texto legible de pequeño tamaño – entre 150 y 200 caracteres.
- Cifrarlo (con salida en modo binario) con CINCO algoritmos simétricos (AES y TDES
obligatorios en modo bloque y flujo y un cifrador de flujo –chacha20 o similar-).
- Descifrarlos y comprobar el resultado
- Explicar el tamaño de los diferentes ficheros cifrados en virtud del tamaño de bloque del cifrador (o no, si se cifra en flujo), y sabiendo que el empleo de sal añade 16 bits de más al inicio del fichero cifrado –Salted__XXXXXXXX-.
- Cifrar un fichero con contraseña y descifrarlo NO con dicha contraseña, sino con su conjunto
equivalente de clave (key), vector de inicialización (iv) y sal (salt).

Para esta parte de la práctica se usará el comando enc para realizar el cifrado y descifrado del fichero usado en la parte anterior. Para ello utilizaremos el **cifrado en bloque** y el **cifrado en flujo**.

El **cifrado en bloque** consiste en dividir el texto plano a cifrar en grupos de bits llamados **bloques** a los que se les aplica una transformación invariable mediante la entrada de una clave secreta. El resultado de este proceso es un bloque de igual tamaño de texto cifrado. Precisamente es este primer tipo de cifrado el que se va a utilizar para cifrar el texto ya ultilizado. Los algoritmos de cifrado en bloque que existen actualmente son **DES**, **TDES** y **AES**.

- **TDES - CBC** <br>
  El primer algoritmo que utilizaremos es el **TDES** (Triple DES) con el modo de operación **CBC**. Concretamente el tipo de TDES que usaremos será el **EDE3**.
  ```
  openssl enc -des-ede3-cbc -p -in ili.txt -out tdes_cbc_ili.cif
  -----------------------------------------------------------
  salt=3275CDCF09B08F95
  key=2AD9528E6F80E33C5D3A523483B9815E645481B91433E0F5
  iv =4636F3970153D513
  -----------------------------------------------------------
  ```
  La clave privada utilizada ha sido "**hola11**" y será la misma clave en todos los comandos que hagamos a partir de ahora. Además se ha añadido la opción **-p** para que muestre la información de la **sal**, la **clave** y el **vector de inicialización**.

  A continuación, usaremos la clave mencionada anteriormente para desencriptar el fichero.

  ```
  openssl enc -des-ede3-cbc -d -k hola11 -in tdes_cbc_ili.cif -out ili_desc.txt
  ```
  Y si leemos el contenido del fichero podremos comprobar que se ha descifrado:
  ```
  cat ili_desc.txt
  Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles
  ```

- **AES - CBC** <br>
  A continuación usaremos el algoritmo **AES** (Advanced Encription Standard) con el modo **CBC**. El algoritmo AES es la evolución del TDES, ya que se está empezando a utilizar cada vez más reemplazando a su versión anterior. 
  ```
  openssl enc -aes-128-cbc -p -in ili.txt -out aes_cbc_ili.cif
  -----------------------------------------------------------
  salt=53015237D6A34F21
  key=FDCC6EAB64626305E9A22E877A61E84A
  iv =B20E1E8AC67042EED38DEB2F29DB955B
  -----------------------------------------------------------
  ```

  En este caso descifraremos el fichero con su **key** e **iv**. Primero le quitaremos al fichero los bits correspondientes a la **sal**.

  ```
  cat aes_cbc_ili.cif | dd ibs=16 obs=16 skip=1 > aes_cbc_ili_sinsal.cif
  ```
  A continuación, ya retirada la sal del fichero, utilizaremos la key e iv que hemos obtenido del comando de cifrado gracias a la opción **-p** para descifrar el fichero.
  ```
  openssl enc -aes-128-cbc -d -in aes_cbc_ili_sinsal.cif -out ili2_desc.txt -K FDCC6EAB64626305E9A22E877A61E84A -iv B20E1E8AC67042EED38DEB2F29DB955B
  ```
  Y si leemos el contenido del fichero podremos comprobar que se ha descifrado:
  ```
  cat ili2_desc.txt
  Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles
  ```

A continuación se utilizarán los algoritmos que aplican el cifrado en flujo. El **cifrado en flujo** se utiliza para situaciones en las que el cifrado en bloque no es apropiado dado que sería ineficiente esperar a llenar un bloque de 64 bits antes de traducir dado que se transmiten pequeñas cantidades de bits a lo largo del tiempo. Un ejemplo de este tipo de casos es el cifrado en conversaciones telefónicas. Para ello, se cifra la información que se va transmitiendo el texto plano bit a bit y cada uno de dichos bits transmitidos se cifra con un dígito de una secuencia pseudoaleatoria. Los algoritmos que se van a mostrar en esta parte de la práctica son los ya vistos AES y TDES en modo de flujo y otro cifrador de flujo llamado **ChaCha20**.

- **DES - OFB** <br>
  A continuación usaremos el algoritmo **DES** ya utilizado en los puntos anteriores con el modo **OFB**. El modo OFB, al igual que los modos **CFB** y **CTR**, permite que cifradores de bloque como DES o AES puedan funcionar como cifradores de flujo. 
  ```
  openssl enc -des-ede3-ofb -p -in ili.txt -out des_ofb_ili.cif
  -----------------------------------------------------------
  salt=3AA7F0A181EE2307
  key=3985DE78D4E02A5A55AF2A51B69A4DDC74FF17E3469EAF73
  iv =92DC3EB750E294D9
  -----------------------------------------------------------
  ```

  A continuación, usaremos la clave "**hola11**" para desencriptar el fichero.

  ```
  openssl enc -des-ede3-ofb -d -k hola11 -in des_ofb_ili.cif -out ili3_desc.txt
  ```
  Y si leemos el contenido del fichero podremos comprobar que se ha descifrado:
  ```
  cat ili3_desc.txt
  Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles
  ```

- **AES - OFB** <br>
  A continuación usaremos el algoritmo **AES** ya utilizado en los puntos anteriores con el modo **CFB**. De igual forma que en el punto anterior, el modo de operación CFB permite al algoritmo AES funcionar como un cifrador de flujo.
  ```
  openssl enc -aes-192-cfb -p -in ili.txt -out aes_cfb_ili.cif
  -----------------------------------------------------------
  salt=4CFDFA2D90CE6E3F
  key=84A98E2C9A632E2F4378E3678994CDCBAE70566C875F9D7C
  iv =A2501ADAFA14EB6A6C98892B99A20281
  -----------------------------------------------------------
  ```

  A continuación, usaremos la clave "**hola11**" para desencriptar el fichero.

  ```
  openssl enc -aes-192-cfb -d -k hola11 -in aes_cfb_ili.cif -out ili4_desc.txt
  ```
  Y si leemos el contenido del fichero podremos comprobar que se ha descifrado:
  ```
  cat ili4_desc.txt
  Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles
  ```

- **Chacha20** <br>
  Para finalizar usaremos el cifrador de flujo **Chacha20**. Este cifrador es más rápido y eficiente que el algoritmo AES y además tiene la ventaja de que permite al usuario buscar en el flujo de claves cualquier posición en tiempo constante.
  ```
  openssl enc -chacha20 -p -in ili.txt -out chacha_ili.cif
  -----------------------------------------------------------
  salt=BDECE1225DD1D995
  key=AFED7B571F197282F9F71609B12C19FEC42418F92B836583A0D8E1401ABDCA99
  iv =F7FEF3EED788BE5FE2CBEDE0736CC4F2
  -----------------------------------------------------------
  ```

  A continuación, usaremos la clave "**hola11**" para desencriptar el fichero.

  ```
  openssl enc -chacha20 -d -k hola11 -in chacha_ili.cif -out ili5_desc.txt
  ```
  Y si leemos el contenido del fichero podremos comprobar que se ha descifrado:
  ```
  cat ili5_desc.txt
  Canta, oh diosa, la cólera del Pelida Aquiles; cólera funesta que causó infinitos males a los aqueos y precipitó al Hades muchas almas valerosas de héroes, a quienes hizo presa de perros y pasto de aves -cumplíase la voluntad de Zeus- desde que se separaron disputando el Atrida, rey de hombres, y el divino Aquiles
  ```

Una vez hemos generado todos los ficheros cifrados podremos comparar el peso de cada uno de ellos. Como podemos comprobar en la imagen de abajo, los ficheros cifrados en flujo pesan menos que los cifrados en bloque, ya que al no tener que rellenar los bits de un bloque entero el espacio ocupado es menor. También podemos denotar una diferencia de tamaños entre el fichero cifrado con TDES en bloque y el fichero cifrado con AES en bloque. Dicha diferencia puede provenir de que el tamaño de bloque del algoritmo AES es de 128 bits, el doble que el del algoritmo TDES que es de 64 bits. 

![Parte 1.2](Parte%201/Parte%201.2.png)


## 1.3 Generación de claves asimétricas (pública-privada) y firmado de resúmenes
- Generar un par de claves asimétricas RSA de 2048 bits.
- Exportar dicho par de claves (pública y privada) en formato PEM (textual) y DER (binario). Utilizar los comandos de conversión de PEM a DER y viceversa.
- Con los dos pares de claves asimétricas creadas, firmar y comprobar la firma del resumen (con SHA-256) de un fichero de texto del apartado anterior.
- Generar dos claves DH con curva elíptica X25519 y demostrar que la combinación pública1-privada2 genera el mismo secreto que la combinación privada1-pública2.

En este apartado haremos uso de la **criptografía asimétrica**. Ésta consiste en la utilización de un par de claves pública y privada que se utilizan para firmar y cifrar ficheros.

Para generar las claves usaremos los comandos del openssl **genpkey** y **pkey**.

Comenzamos generando la clave privada con el comando **genkey**<br>

![Clave Privada](Parte%201/Parte%201.3%20a.1.png)

Aquí podemos ver el contenido del fichero clave_privada.pem
```
cat clave_privada.pem
```
![Clave Privada](Parte%201/cat%20clave_privada.png)

Seguidamente se extraerá la clave pública del fichero recién generado 

#

# Parte 2
## Parte 2.1

#