<p align="center">
  <a href="https://github.com/othneildrew/Best-README-Template">
    <img width="946" alt="Ciberseguridad" src="https://user-images.githubusercontent.com/46871300/125079966-38ef8380-e092-11eb-9b5e-8bd0314d9274.PNG">
  </a>
 
   <h3 align="center">Intalacion de IntelMQ</h3>

  <p align="center">
    Proporcionamos los primeros pasos a los nuevos equipos de manejo de incidentes.
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template"><strong>Explora la guia »</strong></a>
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template">Ver Demo</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Reporta un Bug</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Solicitar función</a>
  </p>
</p>




----
### IntelMQ Overview!


IntelMQ es una solución para equipos de seguridad de TI (CERT y CSIRT, departamentos de abuso de SOC, etc.) para recopilar y procesar fuentes de seguridad (como archivos de registro) mediante un protocolo de cola de mensajes. Es una iniciativa impulsada por la comunidad llamada IHAP (Proyecto de automatización de manejo de incidentes) que fue diseñada conceptualmente por los CERT / CSIRT europeos durante varios eventos de InfoSec. Su objetivo principal es brindar a los respondedores de incidentes una manera fácil de recopilar y procesar inteligencia sobre amenazas, mejorando así los procesos de manejo de incidentes de los CERT.

IntelMQ y la cola de mensajería (corredor) 
IntelMQ usa una cola de mensajes para mover los mensajes entre los bots. Todas las instancias de bot solo pueden procesar un mensaje a la vez, por lo tanto, todos los demás mensajes deben esperar en la cola. Como no todos los bots son igualmente rápidos, los mensajes se “pondrán en cola” de forma natural antes que los más lentos. Además, los analizadores producen muchos eventos con un solo mensaje (el informe) como entrada.

Las siguientes estimaciones asumen que Redis es un agente de mensajería, que es el predeterminado para IntelMQ. Cuando se usa RabbitMQ, los recursos requeridos serán diferentes y RabbitMQ puede manejar la sobrecarga del sistema y, por lo tanto, la escasez de memoria.

Como Redis almacena todos los datos en la memoria, los datos que se procesan en cualquier momento deben caber allí, incluidos los gastos generales. Tenga en cuenta que IntelMQ no almacena ni almacena en caché ningún dato de entrada. Por lo tanto, estas estimaciones solo se refieren al paso de procesamiento, no al almacenamiento.

---

----
### IntelMQ Requisitos de Hardware!


Para un sistema mínimo, estos requisitos son suficientes:

4 GB de RAM

2 CPU

Tamaño de disco de 10 GB

![](https://user-images.githubusercontent.com/87453279/132967583-bc9cc7ad-5ecd-42e5-bbc3-cfc45bcbf3d0.png)

Dependiendo de la entrada de datos, necesitará la vigésima parte del tamaño de los datos de entrada como memoria para el procesamiento.

Cuando utilice la persistencia de Redis , necesitará adicionalmente el doble de memoria para Redis.

Espacio en disco 
El espacio en disco solo es relevante si guarda sus datos en un archivo, lo cual no se recomienda para configuraciones de producción y solo es útil para pruebas y evaluación.

No olvide rotar sus registros o usar syslog, especialmente si usa el nivel de registro “DEBUG”. logrotate se utiliza de forma predeterminada para todas las instalaciones con paquetes deb / rpm. Cuando se utilizan otros medios de instalación (pip, manual), configure la rotación de registros manualmente. Consulte Registro .

Antecedentes de la memoria 
Para la experimentación, utilizamos varios informes de Shadowserver Poodle con fines de demostración, con un total de 120 MB de datos. Todos los números son estimaciones y están redondeados. En memoria, los datos del informe requieren 160 MB. Después del análisis, el uso de memoria aumenta a 850 MB en total, ya que cada línea de datos se almacena como JSON, con información adicional más los datos originales codificados en Base 64. Los pasos de procesamiento adicionales dependen de la configuración, pero puede estimar que los cachés ( para búsquedas y deduplicación) y otra información adicional provocan un aumento de tamaño adicional de aproximadamente 2 veces. Una vez que un conjunto de datos terminó de procesarse en IntelMQ, ya no se almacena en la memoria. Por lo tanto, la memoria solo es necesaria para capturar una carga alta.

Los números anteriores dan como resultado un factor de 14 para el tamaño de los datos de entrada frente a la memoria requerida por Redis. Suponiendo algo de sobrecarga y memoria para los procesos de los bots, un factor de 20 parece sensato.

Para reducir la cantidad de memoria y el tamaño del disco necesarios, puede eliminar opcionalmente el campo de datos sin procesar ; consulte Eliminar datos sin procesar para obtener un mayor rendimiento y menos uso de espacio en las Preguntas frecuentes.

-----



Para xUbuntu 20.04, ejecute lo siguiente:
Tenga en cuenta que el propietario de la clave puede distribuir actualizaciones, paquetes y repositorios en los que su sistema confiará.

```
echo 'deb http://download.opensuse.org/repositories/home:/sebix:/intelmq/xUbuntu_20.04/ /' | sudo tee /etc/apt/sources.list.d/home:sebix:intelmq.list
curl -fsSL https://download.opensuse.org/repositories/home:sebix:intelmq/xUbuntu_20.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home_sebix_intelmq.gpg > /dev/null
```
```
sudo apt update
```
```
sudo apt install intelmq
```
```
sudo apt install intelmq-manager
```

intelmq-api-adduser --user intelmq --password intelmq

```
http://<intelmq_ip>/intelmq-manager
```
