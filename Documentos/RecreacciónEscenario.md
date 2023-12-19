<br>   
  

### Recreacción Escenario 

<br>.
La Tarea la he realizado en un contendor docker en el que he incluído: Laravel 10 con php 8, mysql y nginx  
<br>

<br>
El Escenario que he reproducido en relación al Diseño inseguro se basa en la falta de control en la parte del servidor en cuanto a la subido de archivos.  
Si confiamos en los controles únicamente en el frontend con JavaScript, Jquery, html,..., sin requerir en la parte del servidor esos mismos controles, tendremos un agujero de seguridad importante dado que los controles en el frontend están fuera de nuestro control y pueden ser facilmente saltados
<br> 

En este caso veremos:   
-   No hemos siquiera validado que el campo subida de archivos se requiera por lo que se nos mostrará una pantalla de error desde el servidor que nos dará información con la que podremos buscar vulnerabilidades a explotar   
    - Añadiendo el control al servidor veremos que nos mostrará un mensaje de error cuando no añadamos archivos al campo   
    
    ```
       $request->validate([
            //validar campo requerido
            'file_upload' => 'required',
        ]);   
    ``````

- Por otra parte No hemos delimitado el tipo de archivo a subir por lo que se pueden subir cualquier extensión de archivos, con lo que podremos intentar subir archivos de scripts, shells, ...

     - Añadiendo el control del tipo de archivos permitido así como el tamaño máximo delimitados que se suba cualquier tipo de archivo y tamaño 
    
    ```
       $request->validate([
            //validar Tipo de Archivo y Tamaño Máximo
            'file_upload' => 'required|mimes:pdf,jpg,png|max:2048',
               
        ]);     
    ``````

- Por último vemos que en la url de los archivos subidos se muestra el nombre original del archivo, esto también podría ser un punto a explotar dado que si sabemos que tipo de archivos se suben cual es el formato de nombre de los mismos, ... podremos buscar alguno en específico. 
La configuración para guardar nombre sería:

    ```
        //store original_name
        $filePath= $file->storeAs('',$fileName. '.'.$file->extension(),'public');  
    ``````    
          
    - Le indicamos a Laravel que nos guarde el archivo en la carpeta uploads de public, con esto generará un path ramdom 

    ```
        //store file_path
        $filePath = $file->store('uploads', 'public');
    ```
<br>       


**Enlace Video Youtube** 
- https://www.youtube.com/watch?v=0ItuRnVaBnU

<br>



    






----
<br>  


[![arrow](/Documentos/Imágenes/ic_arrow_back_128_28226.png)](/README.md)