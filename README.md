# Practica del modul MF0493 Implantació d'aplicacions web en entorns internet, intranet i extranet.
##  Parte 1
Para crear la nueva "virtual-box" he modificado el archivo backend.pp de la ruta backend/puppet/manifests/ y he añadido el siguiente codigo:

```
nginx::resource::server{"practica.aplicaciones.web":
  use_default_location => false,
  listen_port => 80,
  www_root  => "/var/www/practica/public",
  index_files => [ 'index.html', 'index.php' ],
}

Qué hará nginx por defecto con este "server"
nginx::resource::location{ 'practica/':
  server      => "practica.aplicaciones.web",
  location => '/',
  # Esta configuración pertenece al dominio que digamos
  ensure         => present,
  # Esta configuración debe quedar escrita siempre
  # en algún fichero de configuración de nginx 
  # por lo tanto puppet creará un archivo en /etc/nginx/conf.d/
  autoindex   => 'off',
  # No se listará el contenido de la carpeta
  location_cfg_append =>{
    try_files => '$uri /index.php$is_args$args' 
  },
  index_files => ['index.php']
}


nginx::resource::location { "practica/index.php":
  server         => "practica.aplicaciones.web",
  location       => '~ ^/index\.php(/|$)',
  ensure         => present,
  fastcgi        => "${url_servidor_aplicaciones}",
  # location of fastcgi 
  fastcgi_split_path => '^(.+\.php)(/.*)$',
  # Info para el servidor de aplicaciones
  #location_cfg_append =>{
  # Info para el servidor de aplicaciones
  # fastcgi_split_path_info => '^(.+\.php)(/.*)$'
  #},
  fastcgi_param  => {
    'APP_ENV'          => 'dev',
    'SCRIPT_FILENAME'  => '$realpath_root$fastcgi_script_name',
    'DOCUMENT_ROOT'    => '$realpath_root'
  }
}
```

En la consola de vagrant usamos la instruccion: `vagrant provision`

Abrir el archivo hosts con un editos de texto como Administrador y añadimos lo siguiente:
`192.168.33.10 practica.aplicaciones.web`

Para desplegar el repositorio del github indicado en la consola de vagrant usamos la siguiente instruccion:
`git clone https://github.com/temple/Aplicaciones_web.git practica`

Para comprobar si se ha cargado la aplicacion en el dominio entramos en el dominion con un navegador.


## Parte 2

Para comprobar las funcionalidades debemos entrar a las diferentes ramas, para ello usaremos la siguiente instruccion en vagrant:

`git checkout nombredelarama`

**Rama Musa-front**

Realizamos un `git checkout Musa-front`

Metodo erroneo ya que no muestra la ruta que solicta el usuario.

En index.php hay un exit en la linea numero 10.

**Rama Walaa-Routing**

Realizamos un `git checkout Walaa-Routing`

Metodo erroneo hay un exit en la linea 4 del index.php.

**Rama Alex-RoutingInterface**

Realizamos un `git checkout Alex_RoutingInterface`


**Rama Jordi-HTML_Response**

Realizamos un `git checkout Jordi-HTML_Response`
Metodo correcto. Entrando en practica.aplicaciones.web nos muetra la informacion solicitada.

**Rama Kyomi_AJAXform**

Realizamos un `git checkout Kyomi_AJAXform`
Metodo erroneo.
Entramos en practica.aplicaciones.web/contact y nos da error.

**Rama Carmen-errors**

Realizamos un `git checkout Carmen-errors`








## Parte 3

Creamos un repositorio en nuestro perfil de github y lo llamamos practica.
Arrastramos el archivo README.md a dicho repositorio.

