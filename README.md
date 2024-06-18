# Bytte-Mobile-ANDROID-Documents-MiiD

<p align="center" >
  <img src="https://www.bytte.com.co/ftpaccess/Varios/CarlosG/ImgDocumentation/logo.png" alt="bytte" title="bytte">
</p>

# Bytte SDK Android

## Actualizacion bytte 17 junio 2024

### Captura de documentos

Soporte de captura de documentos cédula de ciudadanía de hologramas, digital componente físico,
cédula de extranjería, pasaporte, pasaporte con extracción NFC.
se deprecia la captura en formato imagen jpg en las capturas, se recomienda el metodo de imgpback el
cual garantiza temas de seguridad.

## Integración sdk Bytte Android

    Autor Venancio prada.
    Fecha de creación 9 septiembre 2021.
    versión  documentación -> 2.0.0.

# CONFIDENCIALIDAD

La información contenida en el presente documento es CONFIDENCIAL, hace parte del secreto comercial
e industrial de la empresa e implica transmisión de información cuya propiedad corresponde
exclusivamente a BYTTE SAS. En consecuencia, la divulgación o el uso inapropiado de la información
aquí contenida por parte del receptor de la misma, implicarán la aplicación de las normas legales
pertinentes para su debida protección.

# Factores Limitantes

Los factores limitantes para la integración del SDK son:

- Se debe verificar la calidad de la cámara, es recomendable utilizar dispositivos con cámara que
  tengan la característica de “AutoFoco” y flash.
- Se recomiendan cámaras con resolución mayores o iguales a 8 MegaPixeles frontal y 8 MegaPixeles
  para un óptimo rendimiento, “mejor cámara mejora las imágenes”.
- El SDK no funciona sobre dispositivos virtuales, únicamente sobre dispositivos físicos.
- No presenta inconveniente para compilaciones Android 33 y 34.
- Mínima versión soportada Android 23.
- Esta solución ya trabaja sobre androidX por lo cual si el proyecto no lo soporta se debe migrar
  para que la captura de los documentos funcionen.

- **Se debe verificar la calidad de la cámara, es recomendable utilizar dispositivos con cámara que
  tengan la característica de “Auto Foco” habilitada.**
- **Se recomiendan cámaras con resolución mayores o iguales a 8 MegaPixeles para un óptimo
  rendimiento.**
- **Sistemas soportados Android 6.0 nivel de api 23. arquitecturas arm 32,64 bits.**

# Instalación de las librerías

En el archivo gradle build.gradle del proyecto adicionaremos la 'URL' que nos entrega Bytte para la
descarga de los archivos necesarios, al igual que el usuario y token de acceso.

> Las librerías tiene soporte tanto a 32 como a 64 bit en arm.

> Credenciales provistos por bytte para la syncronizacion de las librerias.

> - Url

> - name

> - username

> - password

```gradle

android{
    defaultconfig  después de
    versionName ingresamos
    ndk {abiFilters  "armeabi-v7a", "arm64-v8a"}


repositories {
        maven {
        url 'https://byttetfs.pkgs.visualstudio.com/BytteSDKLibrarys-Android/_packaging/SdkBytteV2/maven/v1'
        name 'SdkBytteV2'
            credentials {
                username ""
                password ""
            }
        }

}
```

# Importamos las Librerias Bytte

En el archivo gradle _build.gradle_ adicionaremos las dependencias.

> Estas son las librerías necesarias para el uso, funcionalidad y captura de documento colombiano.

## Dependencias buid.gradle

```gradle



    implementation 'com.squareup.okhttp3:okhttp:4.9.1'
    implementation("com.squareup.okhttp3:logging-interceptor:4.9.0")
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.jakewharton.timber:timber:4.7.1'
    implementation 'com.jakewharton.retrofit:retrofit2-kotlin-coroutines-adapter:0.9.2'

    implementation 'androidx.appcompat:appcompat:1.4.1'
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'

    implementation 'org.jetbrains.kotlin:kotlin-stdlib:1.6.10'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.5.0'
    implementation 'org.jetbrains.kotlin:kotlin-android-extensions-runtime:1.6.10'



    implementation "androidx.core:core-ktx:1.6.0"
    implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.4.0'
    implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.4.0'


    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.0'
    implementation "com.google.android.material:material:1.5.0"
    implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
    implementation 'com.android.support.constraint:constraint-layout:2.0.4'

    implementation 'com.bytte.microblink:LibBlinkIDCustom:6.2.1'
    implementation 'com.document.bytte:documentBytte:10.0.1'

    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-jdk8:1.2.1"
    implementation "androidx.lifecycle:lifecycle-runtime-ktx:2.4.1"
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"





```

## Manifest

> La configuración del manifest es la siguiente: requiere permisos para el uso de internet, lectura
> y escritura en el dispositivo y cámara.
> Configuración de las actividades Bytte para las capturas.

```
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

 <uses-feature
        android:name="android.hardware.camera"
        android:required="true" />






```

## Captura de Insumos

El app debe solicitar los permisos en tiempo de ejecución antes de usar la funcionalidad y validar
que el permiso fue otorgado.

## Captura Documento

### Inicializar la captura.

> callbacks donde llegara la respuesta correcta "onResponse" o
> errónea "onErrorResponse"

```kotlin
  IDCaptureDocumentBytte.newInstance(
    Activity(),
    new InitializationListener < IDCaptureDocumentBytte >() {
        @Override
        public void onInit(IDCaptureDocumentBytte d) {
            d.setSesionId(sesionId);
            d.setClientID(clientID);
            d.setClientKey(clientKey);
            d.setUrl(url);
            d.setLogtrace(true);
            d.setDocumentNumber("");
            d.seteTcaptureType(ETcaptureType.BACK);
            d.setEcapture(ECapture.COCC);
            d.setTimeOut(timeOut);
            d.capture();
        }
    },
    new BytteResponseListener < String >() {
        @Override
        public void onResponse(String results) {

        }

        @Override
        public void onErrorResponse(String obj) {

        }
    });
```

### Configuración Parámetros la captura.

## onInit(IDCaptureDocumentBytte d)


| Parametro          | Tipo de dato          | Descripcion                                                                                                                       |
|--------------------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| d.ecapture         | Enum -> ECapture      | ECapture.COCC -> cedula hologramas, ECapture.COCCV2 -> cedula digital, ECapture.COEXT -> cedula extrangeria                       |
| d.clientID         | String                | identificador del cliente para el uso el sdk'                                                                                     |
| d.clientKey        | String                | identificador de la llave para el uso del sdk                                                                                     |
| d.seteTcaptureType | Enum -> ETcaptureType | Identifica la captura a generarse ETypecapture.BACK -> reverso, ETypecapture.FRONT -> frontal.                                    |
| d.timeOut          | Integer               | timeout para las capturas tiempo en segundos se recomienda maximo 20                                                              |
| d.url              | String                | url generada por bytte para ambientes y funcionalidades sobre la captura.                                                         |
| d.sesionId         | String                | identificador de la tx o un indentificador unico puedeser un uuid para cifrar las img  y es obligatorio.                          |
| d.dataBackDocument | Boolean               | OCR para el retorno de fecha de expedicion, ciudad de expedicion depende del celular y la camara para que los datos sean exactos. |
| d.capture          |                       | Accion de Inicio para la captura.                                                                                                 

### Respuesta Captura Documentos:

                                                                           |

### Respuesta Cédula Hologramas Reverso:

| Campo            | Tipo    |
|------------------|---------|
| BarcodeBase64    | String  |
| CiudadExpedicion | String  |
| CiudadNacimiento | String  |
| CodigoOperacion  | String  |
| DepartamentoCol  | String  |
| FechaExp         | String  |
| FechaNacimiento  | String  |
| MensajeOriginal  | String  |
| MensajeRetorno   | String  |
| NombresCompletos | String  |
| NumeroCedula     | String  |
| NumeroTarjeta    | String  |
| Pais             | String  |
| PathImagen       | String  |
| PrimerApellido   | String  |
| PrimerNombre     | String  |
| RH               | String  |
| SegundoApellido  | String  |
| SegundoNombre    | String  |
| Sexo             | String  |
| StatusOperacion  | Boolean |
| TipoDedo         | String  |
| TipoDedo2        | String  |
| VersionCedula    | String  |

### Respuesta Cédula Hologramas Frente:

| Campo            | Tipo    |
|------------------|---------|
| Apellidos        | String  |
| CodigoOperacion  | String  |
| MensajeOriginal  | String  |
| MensajeRetorno   | String  |
| Nombres          | String  |
| NumeroCedula     | String  |
| PathImagen       | String  |
| StatusOperacion  | Boolean |

### Respuesta Cédula Digital Reverso:

| Campo              | Valor   |
|--------------------|---------|
| CodigoOperacion    | String  |
| MensajeOriginal    | String  |
| MensajeRetorno     | String  |
| StatusOperacion    | Boolean |
| ReversoMRZ         |         |
| - PathImagen       | String  |
| - Apellidos        | String  |
| - CodigoDocumento  | String  |
| - Edad             | String  |
| - Emisor           | String  |
| - FechaExpiracion  | String  |
| - FechaNacimiento  | String  |
| - MRZ              | String  |
| - Nacionalidad     | String  |
| - Nombres          | String  |
| - NombresCompletos | String  |
| - NumeroDocumento  | String  |
| - OPT              | String  |
| - RUN              | String  |
| - Sexo             | String  |
| - TipoDocumento    | String  |
| - Version1         | String  |

### Respuesta Cédula Digital Frente:

| Campo              | Valor   |
|--------------------|---------|
| MensajeOriginal    | String  |
| MensajeRetorno     | String  |
| StatusOperacion    | Boolean |
| CodigoOperacion    | String  |
| Frente             |         |
| - PathImagen       | String  |
| - Nombres          | String  |
| - NumeroCedula     | String  |

### Respuesta Cédula Extranjería Reverso:

| Campo              | Valor   |
|--------------------|---------|
| CodigoOperacion    | String  |
| MensajeOriginal    | String  |
| MensajeRetorno     | String  |
| StatusOperacion    | Boolean |
| ReversoMRZ         |         |
| - PathImagen       | String  |
| - Apellidos        | String  |
| - CodigoDocumento  | String  |
| - Edad             | String  |
| - Emisor           | String  |
| - FechaExpiracion  | String  |
| - FechaNacimiento  | String  |
| - MRZ              | String  |
| - Nacionalidad     | String  |
| - Nombres          | String  |
| - NombresCompletos | String  |
| - NumeroDocumento  | String  |
| - OPT              | String  |
| - RUN              | String  |
| - Sexo             | String  |
| - TipoDocumento    | String  |
| - Version1         | String  |

### 3.3.2.6 Respuesta Cédula Extranjería Frente:

| Campo              | Valor   |
|--------------------|---------|
| CodigoOperacion    | String  |
| MensajeOriginal    | String  |
| MensajeRetorno     | String  |
| StatusOperacion    | Boolean |
| Frente             |         |
| - PathImagen       | String  |
| - FechaExpiracion  | String  |
| - NumeroCedula     | String  |
| - IsExpired        | Boolean |

### Respuesta Pasaporte:

| Campo                | Valor   |
|----------------------|---------|
| Apellidos            | String  |
| Codigo               | String  |
| CodigoOperacion      | String  |
| Emisor               | String  |
| FechaExpiracion      | String  |
| FechaNacimiento      | String  |
| IdTipoDocumento      | String  |
| IdentificadorProceso | String  |
| MRZ                  | String  |
| Nacionalidad         | String  |
| Nombres              | String  |
| NumeroDocumento      | String  |
| NumeroPersonal       | String  |
| Opt2                 | String  |
| PathImagen           | String  |
| Sexo                 | String  |
| StatusOperacion      | Boolean |
| Version1             | String  |
| IsExpired            | Boolean |

# Código de errores

```
<string name="OK">0000</string><string name="TimeOut">0001</string>
<string name="Cancelado_a_proposito">0002</string>
<string name="Error_No_tiene_permisos_camara">0112</string>



<string name="timeOut">TimeOut</string>
<string name="Canceladoproposito">Cancelado a proposito</string>


------------------------------
Control de cambios
------------------------------

```
