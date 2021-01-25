# OPENCV
MANUAL PARA EL MINIFICADO DE OPENCV.


(Requisitos) Tener instalado los siguientes softwares:

        •	Os: Windows 10
        •	Java: JDK 8
        •	Android Studio 3.1.4 for Windows 64-bit: https://developer.android.com/studio/
        •	Android NDK r16b or greater: https://developer.android.com/ndk/dow...
        •	OpenCV SDK for Android: https://sourceforge.net/projects/open...
        •	Cmake: https://cmake.org/download/
        •	Ant 1.10.1 or greater: http://ant.apache.org/bindownload.cgi
        •	Python 2.6 or greater (but not 3.0
        •	Git for Windows: https://git-scm.com/downloads
        •	mingw32-make: https://sourceforge.net/projects/ming...


1.	Descargar y/o clonar la librería de OpenCV de : https://github.com/opencv/opencv.git
2.	Configuramos las variables de entorno:

        •	Android Studio
        •	Java
        •	ANT
        •	MinGW para compilar aplicaciones nativas en C.

3.	En el CMD  nos úbicamos en la ruta de  **\opencv\platforms** en la carpeta de platform creamos un directorio con el nombre de **build_android_arm** dentro de la carpeta usamos  cmake para configurar el espacio de trabajo.

4.	Estas son las configuraciones a realizar en el CMD:


cmake -G “Unix Makefiles”
-DCMAKE_TOOLCHAIN_FILE=..\platforms\android/android.toolchain.cmake  ..\..
-DANDROID_NDK=C:\Users\jamezcua\android-ndk-r22
-DANT_EXECUTABLE=C:\Users\jamezcua\apache-ant-1.10.9\apache-ant-1.10.9\bin\ant.bat
-DANDROID_NATIVE_API_LEVEL=27
-DANDROID_ABI=armeabi-v7a
-DBUILD_DOCS=OFF
-DBUILD_PERF_TEST=OFF
-DBUILD_TESTS=OFF
-DBUILD_ANDROID_PROJECTS=OFF
-DCMAKE_BUILD_TYPE=Release
-DBUILD_ANDROID_EXAMPLES=OFF
-DWITH_CUDA=OFF
-DBUILD_opencv_java=ON
-DBUILD_ANDROID_PROJECTS=ON
-DANDROID_STL=c++_shared
-DWITH_OPENXR=OFF
-DBUILD_PROTOBUF=OFF
-DWITH_CAROTENE=NO
-DBUILD_opencv_calib3d=OFF
-DBUILD_opencv_video=OFF **

5.	Empezamos a construir con: **mingw32-make**

**NOTA**: Empezaremos a  resolver los errores más comunes.

6.	No existe tal archivo o directorio, vaya al directorio NDK y copie la carpeta de inclusión en la carpeta libcxx

**SOLUCIÓN**

Creamos una carpeta en  **..\android-ndk-r22\sources\cxx-stl\llvm-libc++\libccx**
Y pegamos la carpeta de include de la carpeta **llvm -libc++**.

7.	Al volver a construir con: **mingw32-make** ya no marcara el error de antes y surgirán nuevos como el de cmath  y debmos corregirlos al deshabilitar alguna función no utilizada android-ndk.
Simplemente comentamos las líneas que nos mencionan, en este caso en la clase math.
Guardamos y volvemos a poner el comando **mingw32-make**, detectando nuevos errores.


Aquí nos menciona que existen errores en las funciones y debemos y lo solucionamos al agregar las funciones **MIN MAX Y ABS y con ANDROID_STL** se definen macros en **OpenCV**.
Por lo cual seguimos la ruta donde se encuentran la clase a corregir:
**..\opencv\modules\core\include\opencv2\core\cvdef.h**
Agregamos estas funciones:

Continuando solucionando el error vamos a la clase **matrix_sparse.cpp** Esta la encontramos en : **...\opencv\modules\core\src** en el archivo modificamos

    •	std::max Lo sustituimos por TEMPMAX

Guardamos y volvemos a poner el comando **mingw32-make**, detectando nuevos errores.

8.	Encontraremos error en la función std::max  en el archivo  **base.hpp**. en la ruta: **…\opencv\modules\core\include\opencv2\core** en el archivo modificamos

    •	std::max Lo sustituimos por TEMPMAX

Continuando reparando el problema vamos al archivo  **matrix_sparse.cpp**. Esta la encontramos en : **...\opencv\modules\core\src** en el archivo modificamos

    •	std::max Lo sustituimos por TEMPMAX

Guardamos y volvemos a poner el comando **mingw32-make**, detectando nuevos errores.

9.	Encontraremos error en la función **std::max** en el archivo  **denoise_tvl1.hpp** en la ruta: **…\opencv\modules\photo\src** en el archivo modificamos

   •    std::max Lo sustituimos por TEMPMAX

Guardamos y volvemos a poner el comando **mingw32-make**, detectando nuevos errores.

10.	Encontraremos error en la función **std::isnan** en el archivo **fast_nlmeans_denoising_invoker_commons.hpp**  en la ruta: **…\opencv\modules\photo\src** en el archivo modificamos

**std::isnan Lo sustituimos por TEMPISNAN**

Guardamos y volvemos a poner el comando **mingw32-make**, detectando nuevos errores.

11.	Encontraremos error en la función abs en la clase **npr.hpp** en la ruta: **…\opencv\modules\photo\src**

**abs** Lo sustituimos por **TEMPABS**.

Guardamos y volvemos a poner el comando **mingw32-make**, detectando nuevos errores.

12.	Encontraremos error en la función abs en la clase **seamless_cloning_impl.cpp** en la ruta: **…\opencv\modules\photo\src**

**abs** Lo sustituimos por **TEMPABS**.
Guardamos y volvemos a poner el comando mingw32-make, detectando nuevos errores.

13.	Encontraremos error en la función std::min en la clase roiSelector.cpp en la ruta: **…\opencv\modules\photo\src**

**std::min** Lo sustituimos por **TEMPMIN**.

Guardamos y volvemos a poner el comando **mingw32-make**.

14.	Nota: Al finalizar de compilar el proyecto al 100% , vamos a la ruta: ***…\opencv\modules\java\android_sdk\CMakeFiles\openvc_java_android.dir\***

En el archivo de build.make  modificamos  **./gradlew**  por  **gradlew**
Guardamos y volvemos a poner el comando **mingw32-make**   nos mostrara que termino de ejecutarse correctamente. Ahora ponemos  **mingw32-make** install
Se terminara de ejecutar y ahora si tendremos la biblioteca creada.

15.	Importamos la librería   en new Module >> Library >> agregamos el nombre de la librería creada, el nombre del modulo y el paquete de la librería en este caso asignaremos el nombre de : **org.opencv**

16.	ahora se importara la biblioteca al proyecto  implementation project(“:namelibrary”)

17.	Nos ubicamos la librería anteriormente creada e ingresamos en el explorar a su ruta.
Una vez ubicados en ella vamos a  **..\src\main\java\org\opencv**
En esta ruta vamos a pegar los archivos del siguiente punto.

18.	Vamos a la ruta de donde creamos la biblioteca que en este ejemplo es **build_android_arm** e ingresamos a **build_android_arm\install\sdk\java\src\org\opencv**
Aquí copiamos todos los archivos y los pegamos en la ruta del punto anterior.

19.	Remplazamos la nueva librería nativa de JNI. Por lo tanto ingresamos a la ruta de librería creada en: **build_android_arm\install\sdk\native\libs\armeabi-v7a**  y copiamos el archivo de **libopencv_java4.so**  y lo pegamos en  la ruta del proyecto de prueba  **..\app\src\main\jniLibs**  aquí dentro eliminamos la carpeta de : **arm64-v8a**  e ingresamos a la carpeta de **armeabi-v7a**  y pegamos el archivo **libopencv_java4.so**

20.	Copiamos la librería  **libc++_shared.so** esta la obtenemos del NDK en la ruta :

**\sources\cxx-stl\llvm-libc++\libs\armeabi-v7a**  el archivo antes mencionado lo pegamos en la ruta del punto anterior: **..\app\src\main\ armeabi-v7a**

21.	Sincronizamos el proyecto , hacemos clean and rebuild.

22.	Del proyecto de prueba removemos la carpeta del SDK antigua

23.	Vamos a la ruta de la biblioteca creada **build_android_arm\install**  en esta copiamos la carpeta **SDK**  y lo pegamos en la aplicación de prueba, donde anteriormente estaba la carpeta SDK.

24.	Corremos el proyecto y estará funcionando la librería openCV.

