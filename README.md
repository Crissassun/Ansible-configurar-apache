# Ansible Playbook

### Ansible playbook que nos permite configurar Apache con una web estática en RedHat o Debian.
El objetivo es poder instalar en distribuciones ** Debian o RedHat ** un servidor HTTP Apache, iniciarlo, habilitarlo, configurar una página estática y reiniciar para poder ver los cambios localmente. 

Esto lo logramos haciendo uso de tres roles, el primero llamado Apache en el cual utilizamos para definir en que distribuciones vamos a hacer la instalación, para distribuciones *Debian* haremos uso del módulo apt por el otro lado para distribuciones RedHat haremos uso del módulo yum.

Aunque sé que existe el módulo "package" que autodetecta nuestro OS, prefiero hacerlo con un "when" para determinar que manejador de paquetes usar como ** yum o apt ** por si en el futuro quisiéramos personalizar la instalación de nuestro servidor Apache dependiendo en otros factores.

Si alguien quiere hacerlo usando el módulo package, les dejo el link de la documentación: [Package](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html) o fragmento para hacerlo:

```
# This uses a variable as this changes per distribution.
- name: Remove the apache package
  ansible.builtin.package:
    name: "{{ apache }}"
    state: absent
```

El segundo módulo llamado reloadApache, como su nombre lo indica, hará un reload de todos los unit files de tal manera que podremos ver nuestra página estática.

El último moduló es website, donde almacenaremos nuestro sitio web estático y done, tenemos las instrucciones para poder copiar nuestro sitio web en "/var/ww/html/. en este caso solo será una simple página web.

### Ansible Playbook
Primeramente, debemos clonar este repositorio de la siguiente manera:

```
git clone https://github.com/Crissassun/Ansible-configurar-apache.git
```

### Ejecutar Playbook
En el archivo playbook.yml es donde mando a llamar todos los roles y además especificaremos en que grupos o servidores queremos instalar apache. Yo lo tengo configurado para todos.

Para la ejecución será como cualquier otro playbook, indicando nuestro archivo de inventario previamente configurado correremos lo siguiente:

```
ansible-playbook -i inventory Ansible-configurar-apache/playbook.yml
```

Una vez nuestro playbook término de correr podemos ingresar al servidor o alguno de los servidores donde se hizo la instalación, abrir el navegador por defecto e ingresar nuestra ip local:

Para obtener nuestra ip local ingresamos.
```
hostname -I | awk '{print $1}'
```

Copiamos y pegamos en la url o simplemente escribimos:
```
localhost
```

Y veremos que Apache está instalado, corriendo, habilitado y con nuestra simple página web estática con el texto:

```
My first Apache web deployment using Ansible playbook
```