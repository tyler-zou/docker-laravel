## Laravel docker 系统环境设置


### 步骤

    1. git clone git@github.com:tyler-zou/docker-laravel.git docker-laravel
    2. cd docker-laravel
    3. docker-compose build             // 构建镜像
    4. docker-compose up -d workspace   // 后台启动服务
    5. http://laravel.dev/，即可访问     // 若是虚拟机，需绑定 host 到虚拟机

### docker 的主要操作

1. 查看 docker 信息

        docker info

        Containers: 9
         Running: 7
         Paused: 0
         Stopped: 2
        Images: 98
        Server Version: 1.12.6
        Storage Driver: devicemapper
         Pool Name: docker-253:0-134421665-pool
         Pool Blocksize: 65.54 kB
         Base Device Size: 10.74 GB
         Backing Filesystem: xfs
         Data file: /dev/loop0
         Metadata file: /dev/loop1
         Data Space Used: 7.236 GB
         Data Space Total: 107.4 GB
         Data Space Available: 28.05 GB
         Metadata Space Used: 9.908 MB
         Metadata Space Total: 2.147 GB
         Metadata Space Available: 2.138 GB
         Thin Pool Minimum Free Space: 10.74 GB
         Udev Sync Supported: true
         Deferred Removal Enabled: false
         Deferred Deletion Enabled: false
         Deferred Deleted Device Count: 0
         Data loop file: /var/lib/docker/devicemapper/devicemapper/data
         storage-opt dm.thinpooldev` to specify a custom block storage device.
         Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
         Library Version: 1.02.135-RHEL7 (2016-11-16)
        Logging Driver: journald
        Cgroup Driver: systemd
        Plugins:
         Volume: local
         Network: host null bridge overlay
        Swarm: inactive
        Runtimes: docker-runc runc
        Default Runtime: docker-runc
        Security Options: seccomp selinux
        Kernel Version: 3.10.0-514.10.2.el7.x86_64
        Operating System: CentOS Linux 7 (Core)
        OSType: linux
        Architecture: x86_64
        Number of Docker Hooks: 2
        CPUs: 1
        Total Memory: 1.789 GiB
        Name: tyler
        ID: Z3EJ:FZDI:UUK3:TPQA:A7XF:VG3O:V5GY:MUKO:FN4L:J2W6:GBSF:KNU2
        Docker Root Dir: /var/lib/docker
        Debug Mode (client): false
        Debug Mode (server): false
        Registry: https://index.docker.io/v1/
        Insecure Registries:
         127.0.0.0/8
        Registries: docker.io (secure)       

2. 查看镜像 

        docker images

        docker_nginx                     latest              6ffb1fa6a8bb        4 days ago          181.8 MB
        docker_redis                     latest              b805073db8ca        8 days ago          190.4 MB
        docker_mongodb                   latest              4dc8520d011c        8 days ago          402 MB
        docker_php-fpm                   latest              53d2e9684823        8 days ago          598.1 MB
        docker_workspace                 latest              d210d7dad479        9 days ago          790.3 MB
        docker_memcached                 latest              757b50d5e43d        2 weeks ago         83.17 MB
        docker_mysql                     latest              53da5f55d4ea        2 weeks ago         405.6 MB
        docker.io/tianon/true            latest              464e5072af36        3 weeks ago         125 B
        docker.io/php                    7.0-fpm             ab7230016bda        4 weeks ago         375.2 MB
        docker.io/memcached              latest              83bc4297d84d        4 weeks ago         83.17 MB
        daocloud.io/mysql                latest              7666f75adb6b        8 weeks ago         405.6 MB
        daocloud.io/nginx                latest              cc1b61406712        9 weeks ago         181.8 MB
        daocloud.io/library/rabbitmq     latest              ec798eba2c56        10 weeks ago        178.9 MB
        daocloud.io/library/mongo        latest              0dffc7177b06        10 weeks ago        402 MB
        docker.io/phusion/baseimage      latest              c39664f3d4e5        8 months ago        225.6 MB
        daocloud.io/daocloud/dao-redis   master-init         173a30377d85        22 months ago       190.4 MB

2. 查看所有的 docker 进程

        docker ps -a

        CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                                      NAMES
        0abd96159796        docker_nginx        "nginx"                  4 days ago          Up 4 days               0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   docker_nginx_1
        33c25fae1057        docker_workspace    "/sbin/my_init"          8 days ago          Up 6 days                                                          docker_workspace_1
        966a5ac4397b        docker_php-fpm      "docker-php-entrypoin"   8 days ago          Up 6 days               9000/tcp                                   docker_php-fpm_1
        e3e4800c845c        docker_redis        "redis-server"           8 days ago          Up 6 days               0.0.0.0:6379->6379/tcp                     docker_redis_1
        16a6b7a9708a        tianon/true         "/true"                  8 days ago          Exited (0) 4 days ago                                              docker_volumes_data_1
        cbc2889f672e        docker_mysql        "docker-entrypoint.sh"   8 days ago          Up 6 days               0.0.0.0:3306->3306/tcp                     docker_mysql_1
        045a97ac2d3f        docker_mongodb      "/entrypoint.sh mongo"   8 days ago          Up 6 days               0.0.0.0:27017->27017/tcp                   docker_mongodb_1
        309d134e137b        docker_memcached    "docker-entrypoint.sh"   8 days ago          Up 6 days               0.0.0.0:11211->11211/tcp                   docker_memcached_1
        eceebd6bed0d        tianon/true         "/true"                  8 days ago          Exited (0) 4 days ago                                              docker_volumes_source_1    

4. 进入某个容器

        docker exec -it CONTAINER_ID /bin/bash 

        一般都是进入 workspace 容器(内置多种语言的包管理器)，进行代码的管理。如果是 windows 系统，可以通过共享目录形式挂载到虚拟机内
        挂载命令： vmhgfs-fuse -o allow_other -o nonempty .host:www /var/www

5. 删除全部 exit 的进程

        docker rm $(docker ps -aq)

6. 删除 IMAGE_ID 镜像

        docker rmi IMAGE_ID

7. 操作某个 compose 服务

        docker-compose restart|start|stop service_name

### 工作空间的操作

    进入容器
    docker exec -it 33c25fae1057 /bin/bash
    root@33c25fae1057:/var/www#

    要想使用 PHP 包管理器 Composer 需要先切换至非 root 用户，容器已经内置了 devdock 用户。
    root@33c25fae1057:/var/www# su -  devdock
    devdock@33c25fae1057:~$ 

    切换到项目目录
    devdock@33c25fae1057:~$ cd /var/www/laravel/
    devdock@33c25fae1057:/var/www/laravel$ ls 
    app  artisan  bootstrap  composer.json  composer.lock  config  database  package.json  phpunit.xml  public  readme.md  resources  routes  server.php  storage  tests  vendor  webpack.mix.js

    执行 laravel 命令
    devdock@33c25fae1057:/var/www/laravel$ php artisan list
    Laravel Framework 5.4.16
    
    Usage:
      command [options] [arguments]
    
    Options:
      -h, --help            Display this help message
      -q, --quiet           Do not output any message
      -V, --version         Display this application version
          --ansi            Force ANSI output
          --no-ansi         Disable ANSI output
      -n, --no-interaction  Do not ask any interactive question
          --env[=ENV]       The environment the command should run under
      -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
    
    Available commands:
      clear-compiled       Remove the compiled class file
      down                 Put the application into maintenance mode
      env                  Display the current framework environment
      help                 Displays help for a command
      inspire              Display an inspiring quote
      list                 Lists commands
      migrate              Run the database migrations
      optimize             Optimize the framework for better performance
      serve                Serve the application on the PHP development server
      tinker               Interact with your application
      up                   Bring the application out of maintenance mode
     app
      app:name             Set the application namespace
     auth
      auth:clear-resets    Flush expired password reset tokens
     cache
      cache:clear          Flush the application cache
      cache:forget         Remove an item from the cache
      cache:table          Create a migration for the cache database table
     config
      config:cache         Create a cache file for faster configuration loading
      config:clear         Remove the configuration cache file
     db
      db:seed              Seed the database with records
     event
      event:generate       Generate the missing events and listeners based on registration
     key
      key:generate         Set the application key
     make
      make:auth            Scaffold basic login and registration views and routes
      make:command         Create a new Artisan command
      make:controller      Create a new controller class
      make:event           Create a new event class
      make:job             Create a new job class
      make:listener        Create a new event listener class
      make:mail            Create a new email class
      make:middleware      Create a new middleware class
      make:migration       Create a new migration file
      make:model           Create a new Eloquent model class
      make:notification    Create a new notification class
      make:policy          Create a new policy class
      make:provider        Create a new service provider class
      make:request         Create a new form request class
      make:seeder          Create a new seeder class
      make:test            Create a new test class
     migrate
      migrate:install      Create the migration repository
      migrate:refresh      Reset and re-run all migrations
      migrate:reset        Rollback all database migrations
      migrate:rollback     Rollback the last database migration
      migrate:status       Show the status of each migration
     notifications
      notifications:table  Create a migration for the notifications table
     passport
      passport:client      Create a client for issuing access tokens
      passport:install     Run the commands necessary to prepare Passport for use
      passport:keys        Create the encryption keys for API authentication
     queue
      queue:failed         List all of the failed queue jobs
      queue:failed-table   Create a migration for the failed queue jobs database table
      queue:flush          Flush all of the failed queue jobs
      queue:forget         Delete a failed queue job
      queue:listen         Listen to a given queue
      queue:restart        Restart queue worker daemons after their current job
      queue:retry          Retry a failed queue job
      queue:table          Create a migration for the queue jobs database table
      queue:work           Start processing jobs on the queue as a daemon
     route
      route:cache          Create a route cache file for faster route registration
      route:clear          Remove the route cache file
      route:list           List all registered routes
     schedule
      schedule:run         Run the scheduled commands
     session
      session:table        Create a migration for the session database table
     storage
      storage:link         Create a symbolic link from "public/storage" to "storage/app/public"
     vendor
      vendor:publish       Publish any publishable assets from vendor packages
     view
      view:clear           Clear all compiled view files
    devdock@33c25fae1057:/var/www/laravel$ 

    退出容器
    devdock@33c25fae1057:/var/www/laravel$ exit     // 退出 devdock 用户
    logout
    root@33c25fae1057:/var/www# exit                // 退出容器
    exit
    [root@tyler docker]#

### 其他操作

1. docker 相关操作

        [root@tyler docker]# docker --help
        Usage: docker [OPTIONS] COMMAND [arg...]
               docker [ --help | -v | --version ]
        
        A self-sufficient runtime for containers.
        
        Options:
        
          --config=~/.docker              Location of client config files
          -D, --debug                     Enable debug mode
          -H, --host=[]                   Daemon socket(s) to connect to
          -h, --help                      Print usage
          -l, --log-level=info            Set the logging level
          --tls                           Use TLS; implied by --tlsverify
          --tlscacert=~/.docker/ca.pem    Trust certs signed only by this CA
          --tlscert=~/.docker/cert.pem    Path to TLS certificate file
          --tlskey=~/.docker/key.pem      Path to TLS key file
          --tlsverify                     Use TLS and verify the remote
          -v, --version                   Print version information and quit
        
        Commands:
            attach    Attach to a running container
            build     Build an image from a Dockerfile
            commit    Create a new image from a container's changes
            cp        Copy files/folders between a container and the local filesystem
            create    Create a new container
            diff      Inspect changes on a container's filesystem
            events    Get real time events from the server
            exec      Run a command in a running container
            export    Export a container's filesystem as a tar archive
            history   Show the history of an image
            images    List images
            import    Import the contents from a tarball to create a filesystem image
            info      Display system-wide information
            inspect   Return low-level information on a container, image or task
            kill      Kill one or more running containers
            load      Load an image from a tar archive or STDIN
            login     Log in to a Docker registry.
            logout    Log out from a Docker registry.
            logs      Fetch the logs of a container
            network   Manage Docker networks
            node      Manage Docker Swarm nodes
            pause     Pause all processes within one or more containers
            port      List port mappings or a specific mapping for the container
            ps        List containers
            pull      Pull an image or a repository from a registry
            push      Push an image or a repository to a registry
            rename    Rename a container
            restart   Restart a container
            rm        Remove one or more containers
            rmi       Remove one or more images
            run       Run a command in a new container
            save      Save one or more images to a tar archive (streamed to STDOUT by default)
            search    Search the Docker Hub for images
            service   Manage Docker services
            start     Start one or more stopped containers
            stats     Display a live stream of container(s) resource usage statistics
            stop      Stop one or more running containers
            swarm     Manage Docker Swarm
            tag       Tag an image into a repository
            top       Display the running processes of a container
            unpause   Unpause all processes within one or more containers
            update    Update configuration of one or more containers
            version   Show the Docker version information
            volume    Manage Docker volumes
            wait      Block until a container stops, then print its exit code

2. docker-compose 相关操作

        [root@tyler docker]# docker-compose --help
        Define and run multi-container applications with Docker.
        
        Usage:
          docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
          docker-compose -h|--help
        
        Options:
          -f, --file FILE             Specify an alternate compose file (default: docker-compose.yml)
          -p, --project-name NAME     Specify an alternate project name (default: directory name)
          --verbose                   Show more output
          -v, --version               Print version and exit
          -H, --host HOST             Daemon socket to connect to
        
          --tls                       Use TLS; implied by --tlsverify
          --tlscacert CA_PATH         Trust certs signed only by this CA
          --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
          --tlskey TLS_KEY_PATH       Path to TLS key file
          --tlsverify                 Use TLS and verify the remote
          --skip-hostname-check       Don't check the daemon's hostname against the name specified
                                      in the client certificate (for example if your docker host
                                      is an IP address)
        
        Commands:
          build              Build or rebuild services
          bundle             Generate a Docker bundle from the Compose file
          config             Validate and view the compose file
          create             Create services
          down               Stop and remove containers, networks, images, and volumes
          events             Receive real time events from containers
          exec               Execute a command in a running container
          help               Get help on a command
          kill               Kill containers
          logs               View output from containers
          pause              Pause services
          port               Print the public port for a port binding
          ps                 List containers
          pull               Pull service images
          push               Push service images
          restart            Restart services
          rm                 Remove stopped containers
          run                Run a one-off command
          scale              Set number of containers for a service
          start              Start services
          stop               Stop services
          top                Display the running processes
          unpause            Unpause services
          up                 Create and start containers
          version            Show the Docker-Compose version information
    