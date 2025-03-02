# 🎓 Trabajo Fin de Grado - DNS Cache Viewer  
Configuración de un servidor **BIND9** en Debian para usarlo como DNS en una red local.  

---

## 🛠️ **Paso 1: Instalar BIND9 en Debian**  

> 📌 **NOTA:** Asegúrate de tener privilegios de superusuario antes de ejecutar los comandos.

1. Activa el modo superusuario con:  
```
su
```

2. Para instalar BIND9, ejecuta estos comandos en la máquina virtual con Debian:
```
sudo apt update && sudo apt install bind9 -y
```

3. Verifica que el servicio está corriendo:
```
sudo systemctl status bind9
```

4. Si no está activo, inícialo con:
```bash
sudo systemctl start bind9
sudo systemctl enable bind9
```



## ⚙️ **Paso 2: Configurar BIND9 como Servidor DNS**

5. Edita el archivo de configuración principal:
```bash
sudo nano /etc/bind/named.conf.options
```

6. Busca la sección `options` y ajusta los siguientes parámetros:
```bash
options {
    directory "/var/cache/bind";
    listen-on { any; };  // Asegura que escuche en todas las interfaces
    allow-query { any; };  // Permite consultas desde cualquier equipo
    recursion yes;
    forwarders {
        8.8.8.8;
        8.8.4.4;
    };
};
```

7. Guarda y cierra el archivo (Ctrl + X, luego Y y Enter).

## 🌍 **Paso 3: Configurar una Zona DNS**
>⚠️ **IMPORTANTE**: Usa un dominio que no entre en conflicto con dominios públicos. Se recomienda .local para redes privadas.

8. Edita el archivo de zonas:
```bash
sudo nano /etc/bind/named.conf.local
```

9. Añade la configuración para una zona DNS, por ejemplo, **joanamoros23.local**:

```bash
zone "joanamoros23.local" {
    type master;
    file "/etc/bind/db.joanamoros23.local";
};
```

10. Guarda y cierra el archivo (`Ctrl + X`, luego `Y` y `Enter`).
11. Crea el archivo de la zona:

```bash
sudo cp /etc/bind/db.empty /etc/bind/db.joanamoros23.local
sudo nano /etc/bind/db.joanamoros23.local
```


