
<div style="page-break-after: always;"></div>
<div style="page-break-after: always;"></div>

 
# **Introducción**

En esta práctica se va a profundizar en los conceptos de la Seguridad en la Información utilizando la herramienta OpenSSL. En la primera parte de la misma se usará el comando **openssl** para realizar acciones de cifrado simétrico, resúmenes, claves asimétricas y firmas y cifrados asimétricos. En la segunda parte de la práctica se practicará el envío, recepción y **decodificación manual** de mensajes con el estándar **S/MIME**. Finalmente en la tercera y última parte se creará un certificado **autofirmado** de servidor Web utilizando las herramientas del comando **openssl**.

# Parte 1

En esta primera parte se utilizarán los distintos comandos del openssl para el **cifrado y resumen de ficheros y la generación de claves**.

## Parte 1.1 - Generación y comprobación de Resúmenes
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

## 1.2 - Cifrado Simétrico de documentos
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


## 1.3 - Generación de claves asimétricas (pública-privada) y firmado de resúmenes
- Generar un par de claves asimétricas RSA de 2048 bits.
- Exportar dicho par de claves (pública y privada) en formato PEM (textual) y DER (binario). Utilizar los comandos de conversión de PEM a DER y viceversa.
- Con los dos pares de claves asimétricas creadas, firmar y comprobar la firma del resumen (con SHA-256) de un fichero de texto del apartado anterior.
- Generar dos claves DH con curva elíptica X25519 y demostrar que la combinación pública1-privada2 genera el mismo secreto que la combinación privada1-pública2.

En este apartado haremos uso de la **criptografía asimétrica**. Ésta consiste en la utilización de un par de claves pública y privada que se utilizan para firmar y cifrar ficheros.

Para generar las claves usaremos los comandos del openssl **genpkey** y **pkey**.

Comenzamos generando la clave privada con el comando **genkey**<br>

![Clave Privada](Parte%201/Parte%201.3%20a.1.png)

Seguidamente se extraerá la clave pública del fichero recién generado con el comando **pkey**.

![Clave Pública](Parte%201/Parte%201.3%20a.2.png)

A continuación exportaremos las claves ya generadas desde formato textual (PEM) a formato binario (DER). Usaremos el comando **pkey** del openssl.

![Claves binarias](Parte%201/Parte%201.3%20b.1.png)

Para verificar que la conversión ha sido correcta, convertiremos los ficheros DER a PEM.

![Claves textual convertidas](Parte%201/Parte%201.3%20b.2.png)

Una vez se han generado las claves asimétricas se procederá a firmar y encriptar un resumen de un fichero. Para ello usaremos el fichero **ili.txt** utilizado en los apartados anteriores. Para realizar el resumen usaremos el comando **dgst**.

![Generación de resumen y verificación](Parte%201/Parte%201.3%20c.png)

Como se puede ver en la imagen, la verificación es correcta por lo que el cifrado y firmado con las claves generadas fue un éxito.

A continuación generaremos dos claves de tipo **DH** con **curva elíptica X22519**. Para empezar, generaremos dos pares de claves asimétricas mediante los comandos **genpkey** y **pkey**. El primero lo denominaremos como **k1** y al segundo par **k2**.

![Generación de claves algoritmo X25519](Parte%201/Parte%201.3%20d.1.png)

La finalidad de generar estos dos pares de claves es comprobar que el secreto que se puede derivar de la clave 1 privada con la clave 2 pública es el mismo que el que se genera con la clave 2 privada y la clave 1 pública. Para ello usaremos el comando **pkeyutl** y el parámetro **derive**. 

![Generación de secretos y comprobación](Parte%201/Parte%201.3%20d.2.png)

Para comprobar que ambos ficheros generados son idénticos se ha usado el comando **cmp**, ya que al no mostrar ningún resultado implica que ambos ficheros no tienen ninguna discrepancia en su contenido.

## 1.4 - Cifrado Asimétrico de documentos

En este apartado vamos a reproducir en una secuencia de operaciones el intercambio de información segura entre dos agentes utilizando cifrado simétrico, cifrado asimétrico de las claves simétricas y firma digital (cifrado asimétrico de un resumen del documento original). Para ello, generaremos dos parejas de claves RSA que serán utilizadas por dos agentes (Ana y Berto) de forma que Ana construirá tres ficheros de texto a partir del fichero de texto original, el primero con el fichero cifrado, el segundo con las claves utilizadas y el tercero con la firma digital. Así, primero generaremos las dos claves:
- Generar una pareja de claves RSA en ficheros anapub.pem y anapriv.pem y otra pareja de claves del mismo tipo bertopub.pem y bertopriv.pem.
- Proteger ambas claves privadas con contraseña “anak” y “bertok” respectivamente. Se supone que Ana y Berto han intercambiado sus claves públicas. 

A continuación, realizaremos el trabajo de Ana:
- Cifrar un fichero de texto mensaje.txt de los apartados anteriores con AES-256 en modo CBC, con clave y vector escogidos por el estudiante y sin sal, con salida en formato BASE4 a un fichero llamado cifrado.txt.
- Crear un fichero binario con la concatenación de la clave y el vector utilizados, de nombre claves.txt y cifrarlo con la clave pública bertopub.pem, con salida en formato binario a un fichero claves.bin
- Convertir claves.bin a claves.txt en formato BASE64 (mediante openssl enc -a …)
- Obtener un resumen sha256 del fichero de texto original y firmarlo (es decir, cifrarlo con la clave privada anapriv.pem -clave anak-), con salida en formato BASE64 a un fichero llamado firma.txt.

En este momento, Ana enviaría los tres ficheros obtenidos cifrado.txt, claves.txt y firma.txt (junto a la meta-información relativa a los algoritmos utilizados, cómo se concatenan clave y vector…) a Berto… que seremos nosotros mismos. Actuando como Berto, procederemos a:

- Convertir el fichero claves.txt a fichero binario claves2.bin con openssl enc -a…
- Descifrar el fichero claves2.bin con la clave privada bertopriv.pem (contraseña bertok) y salida a un fichero claves2.txt (debería ser idéntico a claves.txt).
- Extraer de claves2.txt la clave y el vector en formato hexadecimal, siguiendo la metainformación que le envió Ana junto con los tres ficheros.
- Descifrar el fichero cifrado.txt con la clave y el vector obtenidos y salida al fichero mensaje2.txt (que debería ser igual al fichero original mensaje.txt utilizado por Ana).
- Verificar que el fichero firma.txt con la clave pública de Ana, anapub.pem coincide con un resumen sha256 del fichero mensaje2.txt.

Para empezar generamos los pares de claves para Ana y Berto respectivamente:

![Claves ana](Parte%201/Parte%201.4/claves%20ana.png)
![Claves berto](Parte%201/Parte%201.4/claves%20berto.png)

A continuación, actuaremos en el papel de Ana. Para ello cifraremos el fichero que hemos usado en apartados anteriores como **cifrado.txt** con el algoritmo **AES_256** en modo **CBC**.

![Cifrado de texto](Parte%201/Parte%201.4/cifrado.png)

Seguidamente crearemos un fichero con la concatenación de la clave y vector del fichero cifrado y lo pasaremos a formato binario cifrado con la clave pública de Berto.

![Claves formato bin](Parte%201/Parte%201.4/claves%20bin.png)

Y posteriormente lo volveremos a cambiar en formato BASE64 a txt con el comando **enc**.

![Clave de bin a txt](Parte%201/Parte%201.4/clave%20bin%20a%20txt.png)

Finalmente generamos un resumen del fichero original y lo firmaremos con la clave privada de Ana.

![Resumen firmado](Parte%201/Parte%201.4/resumen%20encriptado.png)

Y con esto terminamos con la participación de Ana. A continuación seguiremos con la parte de Berto en la que descifraremos el intercambio de datos cifrados.

La primera acción que tomaremos como Berto será convertir de vuelta el fichero claves.txt a formato binario.

![Claves de bin a txt 2](Parte%201/Parte%201.4/clave%20bin%20a%20txt%20berto.png)

Posteriormente descifraremos el fichero **claves2.bin** usando la clave privada **bertopriv.pem** y el comando **pkeyutl**.

![Descifrar claves2.bin](Parte%201/Parte%201.4/descifrar%20claves2.png)

Como podemos comprobar, el fichero generado es idéntico al claves.txt generado anteriormente por Ana.

A continuación extraemos del fichero claves2.txt el vector de inicialización y la clave. 

![IV y Clave](Parte%201/Parte%201.4/iv%20y%20clave.png)

Con esta información podemos intentar descifrar el fichero **cifrado.txt** que ha pasado Ana.

![Descifrado cifrado.txt](Parte%201/Parte%201.4/decrypt%20cifrado.png)

Como último paso, para verificar que todo este proceso se ha realizado correctamente compararemos el resumen del fichero recién descifrado con el fichero firma.txt enviado por Ana y firmado con la clave pública de Ana.

![Verificación](Parte%201/Parte%201.4/verification.png)
#

# Parte 2
En esta parte de la práctica se pretende que el estudiante domine el manejo y la estructura de correos
electrónicos seguros con el estándar S/MIME, primero como usuario final (utilizando agentes de
correo electrónico convencionales) y seguidamente el análisis y la generación de este tipo de mensajes
de correo electrónico seguro desde línea de comandos.
Para ello:

- El estudiante previamente ha de crear un certificado X.509 y firmarlo con el certificado de Autoridad disponible en la página de la asignatura
- Instalar ambos certificados (usuario y autoridad) en un cliente de correo Thunderbird (versión 100 o superior) y enviar un mensaje firmado y cifrado a la dirección de un compañero de la asignatura, cuyo certificado ha de ser previamente importado en el cliente de correo, después de recibir un mensaje firmado de ese compañero.
- Una vez recibido el correo firmado y cifrado, hemos de decodificar manualmente, con la ayuda
de la utilidad “openssl smime” el texto del mensaje recibido, empleando nuestro certificado original
(con la clave privada) y el certificado del compañero.

## 2.1 - Creación de nuestro certificado temporal
En este apartado se nos pide **generar nuestro certificado personal** a partir de un certificado raíz que tendremos que descargar de la página de la asignatura.
- Descargar certificadoRaiz.crt y certificadoRaiz.key de la página de la asignatura

    Descargamos desde la página de la asignatura los ficheros **certificadoRaiz.crt** y **certificadoRaiz.key**. 
    ![Comprobación de certificados](Parte%202/fich.png)

- Crear la clave de nuestro certificado (openssl genpkey ...) certificadoPersonal.key

    Haciendo uso del comando **genpkey** generamos la clave de nuestro certificado.
    ![Generación de clave personal](Parte%202/Parte%202.1.2%20b.png)

- Crear el CSR de nuestro certificado (openssl req –new ...) certificadoPersonal.csr
    
    En este caso haremos uso del comando **req** para generar nuestro certificado. Como podremos comprobar en la figura inmediatamente inferior hemos rellenado los datos que se nos solicita con nuestra información personal, además de introducir la contraseña de nuestro certificado que será *hola11*.
    ![Creación CSR](Parte%202/Parte%202.1.2%20c.png)

- Crear nuestro certificado personal a partir del CSR, el certificado raíz y su clave, para ello habrá que introducir la contraseña SEGURIDAD que protege la clave de la raíz, el comando será del tipo (openssl x509 –req –days 365 ...) certificadoPersonal.crt

    Usamos el comando **x509** para crear con toda la información descargada y generada nuestro certificado personal. Para ello tendremos que hacer uso de los ficheros correspondientes al certificado Raíz e introducir la contraseña SEGURIDAD proporcionada por el enunciado.
    ![Creación certificado personal](Parte%202/Parte%202.1.2%20d.png)

    Una vez hayamos generado nuestro certificado personal, podremos verificarlo con el comando **verify** para comprobar que el proceso es correcto.

    ![Validación](Parte%202/Parte%202.1.2%20d%20(verificar).png)

- El último apartado de la Wiki, 6.6: PEM a PKCS#12 explica cómo crear un  fichero “inter.p12” a partir de certificadoIntermedio.crt y certificadoIntermedio.key, tendremos que hacer un comando similar, cambiando “Intermedio” por “Personal” para crear un fichero llamado certificadoPersonal.p12. Atención: el comando nos pedirá la clave que protege certificadoPersonal y a continuación otra clave (podría ser la misma) para proteger el fichero de salida, certificadoPersonal.p12. El comando será del tipo (openssl pkcs12 export ………) certificadoPersonal.p12

    Usando el comando **pkcs12** podremos generar un fichero de formato .p12 para     poder importarlo en el cliente de Mozilla Thunderbird.
    ![Generar fichero .p12](Parte%202/Parte%202.1.2%20e.png)

## 2.2 - Instalación de los certificados en el cliente de correo e intercambio de certificados y correo firmado y cifrado con otro compañero

- Una vez creado nuestro certificado digital, hemos de configurar el cliente de correo electrónico Mozilla Thunderbird para que pueda firmar y cifrar mensajes con dicho certificado.

  Para ello se tendrá que ingresar el correo electrónico correspondiente (en este caso se ha escogido utilizar un correo electrónico personal) en el apartado de **Configuración de la cuenta** y 

- En este cliente de correo electrónico hemos de instalar tanto la parte pública del certificado raíz (fichero certificadoRaiz.crt), como nuestro certificado personal completo (fichero certificadoPersonal.p12) con la contraseña que le pusimos.

  Lo primero que se tendrá que hacer será importar el **certificado raíz** que hemos descargado de la página de la asignatura. Para ello habrá que dirigirse al apartado de **Configuración de la Cuenta** y seleccionar **Administrar certificados S/MIME** en la pestaña de cifrado de extremo a extremo

  ![Configuración del cliente certificado raíz](Parte%202/Parte%202.2.2%20(certificado%20raiz).png)
  ![Configuración del cliente certificado personal](Parte%202/Parte%202.2.2%20(certificado%20personal).png)

- Con ambos certificados ya instalados, hay que configurar nuestra cuenta de correo electrónico para que utilice nuestro certificado personal.


- Una vez configurado e instalado el certificado, podremos FIRMAR los mensajes que enviemos, pero para cifrar correo hacia un destinatario, es necesario disponer previamente de SU certificado personal.


- Enviar un mensaje con las opciones de firmado y cifrado activadas. 
#