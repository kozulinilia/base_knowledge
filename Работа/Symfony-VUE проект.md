# Symfony-VUE проект

## Структура

- bin
    - console
        
        ```jsx
        #!/usr/bin/env php
        <?php
        
        use App\Kernel;
        use Symfony\Bundle\FrameworkBundle\Console\Application;
        use Symfony\Component\Console\Input\ArgvInput;
        use Symfony\Component\ErrorHandler\Debug;
        
        if (!in_array(PHP_SAPI, ['cli', 'phpdbg', 'embed'], true)) {
            echo 'Warning: The console should be invoked via the CLI version of PHP, not the '.PHP_SAPI.' SAPI'.PHP_EOL;
        }
        
        set_time_limit(0);
        
        require dirname(__DIR__).'/vendor/autoload.php';
        
        if (!class_exists(Application::class)) {
            throw new LogicException('You need to add "symfony/framework-bundle" as a Composer dependency.');
        }
        
        $input = new ArgvInput();
        if (null !== $env = $input->getParameterOption(['--env', '-e'], null, true)) {
            putenv('APP_ENV='.$_SERVER['APP_ENV'] = $_ENV['APP_ENV'] = $env);
        }
        
        if ($input->hasParameterOption('--no-debug', true)) {
            putenv('APP_DEBUG='.$_SERVER['APP_DEBUG'] = $_ENV['APP_DEBUG'] = '0');
        }
        
        require dirname(__DIR__).'/config/bootstrap.php';
        
        if ($_SERVER['APP_DEBUG']) {
            umask(0000);
        
            if (class_exists(Debug::class)) {
                Debug::enable();
            }
        }
        
        $kernel = new Kernel($_SERVER['APP_ENV'], (bool) $_SERVER['APP_DEBUG']);
        $application = new Application($kernel);
        $application->run($input);
        ```
        
    - phpunit
        
        ```jsx
        #!/usr/bin/env php
        <?php
        
        if (!ini_get('date.timezone')) {
            ini_set('date.timezone', 'UTC');
        }
        
        if (is_file(dirname(__DIR__).'/vendor/phpunit/phpunit/phpunit')) {
            define('PHPUNIT_COMPOSER_INSTALL', dirname(__DIR__).'/vendor/autoload.php');
            require PHPUNIT_COMPOSER_INSTALL;
            PHPUnit\TextUI\Command::main();
        } else {
            if (!is_file(dirname(__DIR__).'/vendor/symfony/phpunit-bridge/bin/simple-phpunit.php')) {
                echo "Unable to find the `simple-phpunit.php` script in `vendor/symfony/phpunit-bridge/bin/`.\n";
                exit(1);
            }
        
            require dirname(__DIR__).'/vendor/symfony/phpunit-bridge/bin/simple-phpunit.php';
        }
        ```
        
- config
    - packages
        - dev
            - debug.yml
                
                ```jsx
                debug:
                    # Forwards VarDumper Data clones to a centralized server allowing to inspect dumps on CLI or in your browser.
                    # See the "server:dump" command to start a new server.
                    dump_destination: "tcp://%env(VAR_DUMPER_SERVER)%"
                ```
                
            - monilog.yml
                
                ```jsx
                monolog:
                    handlers:
                        main:
                            type: stream
                            path: "%kernel.logs_dir%/%kernel.environment%.log"
                            level: debug
                            channels: ["!event"]
                        # uncomment to get logging in your browser
                        # you may have to allow bigger header sizes in your Web server configuration
                        #firephp:
                        #    type: firephp
                        #    level: info
                        #chromephp:
                        #    type: chromephp
                        #    level: info
                        console:
                            type: console
                            process_psr_3_messages: false
                            channels: ["!event", "!doctrine", "!console"]
                ```
                
            - web_profiler.yml
                
                ```jsx
                web_profiler:
                    toolbar: true
                    intercept_redirects: false
                
                framework:
                    profiler: { only_exceptions: false }
                ```
                
        - prod
            
            deprecations.yml
            
            - deprecations.yml
                
                ```jsx
                # As of Symfony 5.1, deprecations are logged in the dedicated "deprecation" channel when it exists
                #monolog:
                #    channels: [deprecation]
                #    handlers:
                #        deprecation:
                #            type: stream
                #            channels: [deprecation]
                #            path: php://stderr
                ```
                
            - doctrine.yaml
                
                ```jsx
                doctrine:
                    orm:
                        auto_generate_proxy_classes: false
                        query_cache_driver:
                            type: pool
                            pool: doctrine.system_cache_pool
                        result_cache_driver:
                            type: pool
                            pool: doctrine.result_cache_pool
                
                framework:
                    cache:
                        pools:
                            doctrine.result_cache_pool:
                                adapter: cache.app
                            doctrine.system_cache_pool:
                                adapter: cache.system
                ```
                
            - monolog.yaml
                
                ```jsx
                monolog:
                    handlers:
                        main:
                            type: fingers_crossed
                            action_level: error
                            handler: nested
                            excluded_http_codes: [404, 405]
                            buffer_size: 50 # How many messages should be saved? Prevent memory leaks
                        nested:
                            type: stream
                            path: php://stderr
                            level: debug
                            formatter: monolog.formatter.json
                        console:
                            type: console
                            process_psr_3_messages: false
                            channels: ["!event", "!doctrine"]
                ```
                
            - routing.yaml
                
                ```jsx
                framework:
                    router:
                        strict_requirements: null
                ```
                
        - test
            - doctrine.yaml
                
                ```jsx
                doctrine:
                    dbal:
                        # "TEST_TOKEN" is typically set by ParaTest
                        dbname_suffix: '_test%env(default::TEST_TOKEN)%'
                ```
                
            - framework.yaml
                
                ```jsx
                framework:
                    test: true
                    session:
                        storage_id: session.storage.mock_file
                ```
                
            - monolog.yaml
                
                ```jsx
                monolog:
                    handlers:
                        main:
                            type: fingers_crossed
                            action_level: error
                            handler: nested
                            excluded_http_codes: [404, 405]
                            channels: ["!event"]
                        nested:
                            type: stream
                            path: "%kernel.logs_dir%/%kernel.environment%.log"
                            level: debug
                ```
                
            - twig.yaml
                
                ```jsx
                twig:
                    strict_variables: true
                ```
                
            - validator.yaml
                
                ```jsx
                framework:
                    validation:
                        not_compromised_password: false
                ```
                
            - web_profiler.yaml
                
                ```jsx
                web_profiler:
                    toolbar: false
                    intercept_redirects: false
                
                framework:
                    profiler: { collect: false }
                ```
                
        - cache.yaml
            
            ```jsx
            framework:
                cache:
                    # Unique name of your app: used to compute stable namespaces for cache keys.
                    #prefix_seed: your_vendor_name/app_name
            
                    # The "app" cache stores to the filesystem by default.
                    # The data in this cache should persist between deploys.
                    # Other options include:
            
                    # Redis
                    #app: cache.adapter.redis
                    #default_redis_provider: redis://localhost
            
                    # APCu (not recommended with heavy random-write workloads as memory fragmentation can cause perf issues)
                    #app: cache.adapter.apcu
            
                    # Namespaced pools use the above "app" backend by default
                    #pools:
                        #my.dedicated.cache: null
            ```
            
        - doctrine.yaml
            
            ```jsx
            doctrine:
                dbal:
                    url: '%env(resolve:DATABASE_URL)%'
            
                    # IMPORTANT: You MUST configure your server version,
                    # either here or in the DATABASE_URL env var (see .env file)
                    #server_version: '13'
                orm:
                    auto_generate_proxy_classes: true
                    naming_strategy: doctrine.orm.naming_strategy.underscore_number_aware
                    auto_mapping: true
                    mappings:
                        App:
                            is_bundle: false
                            type: annotation
                            dir: '%kernel.project_dir%/src/Entity'
                            prefix: 'App\Entity'
                            alias: App
            ```
            
        - doctrine_migrations.yaml
            
            ```jsx
            doctrine:
                dbal:
                    url: '%env(resolve:DATABASE_URL)%'
            
                    # IMPORTANT: You MUST configure your server version,
                    # either here or in the DATABASE_URL env var (see .env file)
                    #server_version: '13'
                orm:
                    auto_generate_proxy_classes: true
                    naming_strategy: doctrine.orm.naming_strategy.underscore_number_aware
                    auto_mapping: true
                    mappings:
                        App:
                            is_bundle: false
                            type: annotation
                            dir: '%kernel.project_dir%/src/Entity'
                            prefix: 'App\Entity'
                            alias: App
            ```
            
        - framework.yaml
            
            ```jsx
            # see https://symfony.com/doc/current/reference/configuration/framework.html
            framework:
                secret: '%env(APP_SECRET)%'
                #csrf_protection: true
                #http_method_override: true
            
                # Enables session support. Note that the session will ONLY be started if you read or write from it.
                # Remove or comment this section to explicitly disable session support.
                session:
                    handler_id: null
                    cookie_secure: auto
                    cookie_samesite: lax
            
                #esi: true
                #fragments: true
                php_errors:
                    log: true
            ```
            
        - mailer.yaml
            
            ```jsx
            framework:
                mailer:
                    dsn: '%env(MAILER_DSN)%'
            ```
            
        - routing.yaml
            
            ```jsx
            framework:
                router:
                    utf8: true
            ```
            
        - security.yaml
            
            ```jsx
            security:
            # https://symfony.com/doc/current/security.html#where-do-users-come-from-user-providers
            providers:
                    users_in_memory: { memory: null }
                firewalls:
                    dev:
                        pattern: ^/(_(profiler|wdt)|css|images|js)/
                        security: false
                    main:
                        anonymous: lazy
                        provider: users_in_memory
            
            # activate different ways to authenticate
                        # https://symfony.com/doc/current/security.html#firewalls-authentication
            
                        # https://symfony.com/doc/current/security/impersonating_user.html
                        # switch_user: true
            
                # Easy way to control access for large sections of your site
                # Note: Only the *first* access control that matches will be used
            access_control:
            # - { path: ^/admin, roles: ROLE_ADMIN }
                    # - { path: ^/profile, roles: ROLE_USER }
            ```
            
        - sensio_framework_extra.yaml
            
            ```jsx
            sensio_framework_extra:
                router:
                    annotations: false
            ```
            
        - translation.yaml
            
            ```
            framework:
                default_locale: en
                translator:
                    default_path: '%kernel.project_dir%/translations'
                    fallbacks:
                        - en
            
            ```
            
        - twig.yaml
            
            ```
            twig:
                default_path: '%kernel.project_dir%/templates'
                debug: '%kernel.debug%'
                strict_variables: '%kernel.debug%'
                exception_controller: null
            ```
            
        - validator.yaml
            
            ```
            framework:
                validation:
                    email_validation_mode: html5
            
            # Enables validator auto-mapping support.
                    # For instance, basic validation constraints will be inferred from Doctrine's metadata.
                    #auto_mapping:
                    #    App\Entity\: []
            ```
            
    - routes
        - dev
            
            framework.yaml
            
            ```
            _errors:
                resource: '@FrameworkBundle/Resources/config/routing/errors.xml'
                prefix: /_error
            ```
            
            web_profiler.yaml
            
            ```
            web_profiler_wdt:
                resource: '@WebProfilerBundle/Resources/config/routing/wdt.xml'
                prefix: /_wdt
            
            web_profiler_profiler:
                resource: '@WebProfilerBundle/Resources/config/routing/profiler.xml'
                prefix: /_profiler
            ```
            
        
        annotations.yaml
        
        ```
        web_profiler_wdt:
            resource: '@WebProfilerBundle/Resources/config/routing/wdt.xml'
            prefix: /_wdt
        
        web_profiler_profiler:
            resource: '@WebProfilerBundle/Resources/config/routing/profiler.xml'
            prefix: /_profiler
        ```
        
    - bootstrap.php
        
        ```php
        <?php
        
        use Symfony\Component\Dotenv\Dotenv;
        
        require dirname(__DIR__).'/vendor/autoload.php';
        
        if (!class_exists(Dotenv::class)) {
            throw new LogicException('Please run "composer require symfony/dotenv" to load the ".env" files configuring the application.');
        }
        
        // Load cached env vars if the .env.local.php file exists
        // Run "composer dump-env prod" to create it (requires symfony/flex >=1.2)
        if (is_array($env = @include dirname(__DIR__).'/.env.local.php') && (!isset($env['APP_ENV']) || ($_SERVER['APP_ENV'] ?? $_ENV['APP_ENV'] ?? $env['APP_ENV']) === $env['APP_ENV'])) {
            (new Dotenv(false))->populate($env);
        } else {
            // load all the .env files
            (new Dotenv(false))->loadEnv(dirname(__DIR__).'/.env');
        }
        
        $_SERVER += $_ENV;
        $_SERVER['APP_ENV'] = $_ENV['APP_ENV'] = ($_SERVER['APP_ENV'] ?? $_ENV['APP_ENV'] ?? null) ?: 'dev';
        $_SERVER['APP_DEBUG'] = $_SERVER['APP_DEBUG'] ?? $_ENV['APP_DEBUG'] ?? 'prod' !== $_SERVER['APP_ENV'];
        $_SERVER['APP_DEBUG'] = $_ENV['APP_DEBUG'] = (int) $_SERVER['APP_DEBUG'] || filter_var($_SERVER['APP_DEBUG'],FILTER_VALIDATE_BOOLEAN) ? '1' : '0';
        ```
        
    - bundles.php
        
        ```php
        <?php
        
        return [
            Symfony\Bundle\FrameworkBundle\FrameworkBundle::class => ['all' => true],
            Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle::class => ['all' => true],
            Symfony\Bundle\TwigBundle\TwigBundle::class => ['all' => true],
            Symfony\Bundle\WebProfilerBundle\WebProfilerBundle::class => ['dev' => true, 'test' => true],
            Symfony\Bundle\MonologBundle\MonologBundle::class => ['all' => true],
            Symfony\Bundle\DebugBundle\DebugBundle::class => ['dev' => true],
            Symfony\Bundle\MakerBundle\MakerBundle::class => ['dev' => true],
            Doctrine\Bundle\DoctrineBundle\DoctrineBundle::class => ['all' => true],
            Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle::class => ['all' => true],
            Symfony\Bundle\SecurityBundle\SecurityBundle::class => ['all' => true],
            Twig\Extra\TwigExtraBundle\TwigExtraBundle::class => ['all' => true],
        ];
        ```
        
    - preload.php
        
        ```
        <?php
        
        if (file_exists(dirname(__DIR__).'/var/cache/prod/srcApp_KernelProdContainer.preload.php')) {
            require dirname(__DIR__).'/var/cache/prod/srcApp_KernelProdContainer.preload.php';
        }
        
        if (file_exists(dirname(__DIR__).'/var/cache/prod/App_KernelProdContainer.preload.php')) {
            require dirname(__DIR__).'/var/cache/prod/App_KernelProdContainer.preload.php';
        }
        ```
        
    - routes.yaml
        
        ```php
        #index:
        #    path: /
        #    controller: App\Controller\DefaultController::index
        ```
        
    - services.yaml
        
        ```php
        # This file is the entry point to configure your own services.
        # Files in the packages/ subdirectory configure your dependencies.
        
        # Put parameters here that don't need to change on each machine where the app is deployed
        # https://symfony.com/doc/current/best_practices/configuration.html#application-related-configuration
        parameters:
        
        services:
        # default configuration for services in *this* file
        _defaults:
                autowire: true# Automatically injects dependencies in your services.
        autoconfigure: true# Automatically registers your services as commands, event subscribers, etc.
        
            # makes classes in src/ available to be used as services
            # this creates a service per class whose id is the fully-qualified class name
        App\:
                resource: '../src/'
                exclude:
                    - '../src/DependencyInjection/'
                    - '../src/Entity/'
                    - '../src/Kernel.php'
                    - '../src/Tests/'
        
        # controllers are imported separately to make sure services can be injected
            # as action arguments even if you don't extend any base controller class
        App\Controller\:
                resource: '../src/Controller/'
                tags: ['controller.service_arguments']
        
        # add more service definitions when explicit configuration is needed
            # please note that last definitions always *replace* previous ones
        ```
        
- public
    - index.php
        
        ```php
        <?php
        
        use App\Kernel;
        use Symfony\Component\ErrorHandler\Debug;
        use Symfony\Component\HttpFoundation\Request;
        
        require dirname(__DIR__).'/config/bootstrap.php';
        
        if ($_SERVER['APP_DEBUG']) {
            umask(0000);
        
            Debug::enable();
        }
        
        if ($trustedProxies = $_SERVER['TRUSTED_PROXIES'] ?? false) {
            Request::setTrustedProxies(explode(',', $trustedProxies), Request::HEADER_X_FORWARDED_FOR| Request::HEADER_X_FORWARDED_PORT| Request::HEADER_X_FORWARDED_PROTO);
        }
        
        if ($trustedHosts = $_SERVER['TRUSTED_HOSTS'] ?? false) {
            Request::setTrustedHosts([$trustedHosts]);
        }
        
        $kernel = new Kernel($_SERVER['APP_ENV'], (bool) $_SERVER['APP_DEBUG']);
        $request = Request::createFromGlobals();
        $response = $kernel->handle($request);
        $response->send();
        $kernel->terminate($request, $response);
        ```
        
- src
    - Controller
    - Entity
    - Repository
    - Kernel.php
        
        ```php
        <?php
        
        namespace App;
        
        use Symfony\Bundle\FrameworkBundle\Kernel\MicroKernelTrait;
        use Symfony\Component\Config\Loader\LoaderInterface;
        use Symfony\Component\Config\Resource\FileResource;
        use Symfony\Component\DependencyInjection\ContainerBuilder;
        use Symfony\Component\HttpKernel\Kernel as BaseKernel;
        use Symfony\Component\Routing\RouteCollectionBuilder;
        
        class Kernel extendsBaseKernel
        {
            use MicroKernelTrait;
        
            private constCONFIG_EXTS= '.{php,xml,yaml,yml}';
        
            public function registerBundles(): iterable
            {
                $contents = require $this->getProjectDir().'/config/bundles.php';
                foreach ($contents as $class => $envs) {
                    if ($envs[$this->environment] ?? $envs['all'] ?? false) {
                        yield new $class();
                    }
                }
            }
        
            public function getProjectDir(): string
            {
                return \dirname(__DIR__);
            }
        
            protected function configureContainer(ContainerBuilder $container, LoaderInterface $loader): void
            {
                $container->addResource(new FileResource($this->getProjectDir().'/config/bundles.php'));
                $container->setParameter('container.dumper.inline_class_loader', \PHP_VERSION_ID< 70400 || $this->debug);
                $container->setParameter('container.dumper.inline_factories', true);
                $confDir = $this->getProjectDir().'/config';
        
                $loader->load($confDir.'/{packages}/*'.self::CONFIG_EXTS, 'glob');
                $loader->load($confDir.'/{packages}/'.$this->environment.'/*'.self::CONFIG_EXTS, 'glob');
                $loader->load($confDir.'/{services}'.self::CONFIG_EXTS, 'glob');
                $loader->load($confDir.'/{services}_'.$this->environment.self::CONFIG_EXTS, 'glob');
            }
        
            protected function configureRoutes(RouteCollectionBuilder $routes): void
            {
                $confDir = $this->getProjectDir().'/config';
        
                $routes->import($confDir.'/{routes}/'.$this->environment.'/*'.self::CONFIG_EXTS, '/', 'glob');
                $routes->import($confDir.'/{routes}/*'.self::CONFIG_EXTS, '/', 'glob');
                $routes->import($confDir.'/{routes}'.self::CONFIG_EXTS, '/', 'glob');
            }
        }
        
        ```
        
- templates
    - base.html.twig
        
        ```php
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="UTF-8">
                <title>{% block title %}Welcome!{% endblock %}</title>
                {# Run `composer require symfony/webpack-encore-bundle`
                   and uncomment the following Encore helpers to start using Symfony UX #}
                {% block stylesheets %}
                    {#{{ encore_entry_link_tags('app') }}#}
                {% endblock %}
        
                {% block javascripts %}
                    {#{{ encore_entry_script_tags('app') }}#}
                {% endblock %}
            </head>
            <body>
                {% block body %}{% endblock %}
            </body>
        </html>
        ```
        
- tests
    - bootstrap.php
        
        ```
        <?php
        
        use Symfony\Component\Dotenv\Dotenv;
        
        require dirname(__DIR__).'/vendor/autoload.php';
        
        if (file_exists(dirname(__DIR__).'/config/bootstrap.php')) {
            require dirname(__DIR__).'/config/bootstrap.php';
        } elseif (method_exists(Dotenv::class, 'bootEnv')) {
            (new Dotenv())->bootEnv(dirname(__DIR__).'/.env');
        }
        ```
        
- .env
    
    ```
    # In all environments, the following files are loaded if they exist,
    # the latter taking precedence over the former:
    #
    #  * .env                contains default values for the environment variables needed by the app
    #  * .env.local          uncommitted file with local overrides
    #  * .env.$APP_ENV       committed environment-specific defaults
    #  * .env.$APP_ENV.local uncommitted environment-specific overrides
    #
    # Real environment variables win over .env files.
    #
    # DO NOT DEFINE PRODUCTION SECRETS IN THIS FILE NOR IN ANY OTHER COMMITTED FILES.
    #
    # Run "composer dump-env prod" to compile .env files for production use (requires symfony/flex >=1.2).
    # https://symfony.com/doc/current/best_practices.html#use-environment-variables-for-infrastructure-configuration
    
    ###> symfony/framework-bundle ###
    APP_ENV=dev
    APP_SECRET=149f615e93b3b7c4a9022e3646c4e187
    #TRUSTED_PROXIES=127.0.0.0/8,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16
    #TRUSTED_HOSTS='^(localhost|example\.com)$'
    ###< symfony/framework-bundle ###
    
    ###> symfony/mailer ###
    # MAILER_DSN=smtp://localhost
    ###< symfony/mailer ###
    
    ###> doctrine/doctrine-bundle ###
    # Format described at https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/configuration.html#connecting-using-a-url
    # IMPORTANT: You MUST configure your server version, either here or in config/packages/doctrine.yaml
    #
    # DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
    # DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=5.7"
    DATABASE_URL="postgresql://symfony:ChangeMe@127.0.0.1:5432/app?serverVersion=13&charset=utf8"
    ###< doctrine/doctrine-bundle ###
    ```
    
- .env.test
    
    ```
    # define your env variables for the test env here
    KERNEL_CLASS='App\Kernel'
    APP_SECRET='$ecretf0rt3st'
    SYMFONY_DEPRECATIONS_HELPER=999999
    PANTHER_APP_ENV=panther
    PANTHER_ERROR_SCREENSHOT_DIR=./var/error-screenshots
    ```
    
- .gitignore
    
    ```
    ###> symfony/framework-bundle ###
    /.env.local
    /.env.local.php
    /.env.*.local
    /config/secrets/prod/prod.decrypt.private.php
    /public/bundles/
    /var/
    /vendor/
    ###< symfony/framework-bundle ###
    
    ###> symfony/phpunit-bridge ###
    .phpunit.result.cache
    /phpunit.xml
    ###< symfony/phpunit-bridge ###
    
    ###> phpunit/phpunit ###
    /phpunit.xml
    .phpunit.result.cache
    ###< phpunit/phpunit ###
    ```
    
- composer.json
    
    ```
    {
        "type": "project",
        "license": "proprietary",
        "require": {
            "php": ">=7.1.3",
            "ext-ctype": "*",
            "ext-iconv": "*",
            "composer/package-versions-deprecated": "1.11.99.4",
            "doctrine/annotations": "^1.0",
            "doctrine/doctrine-bundle": "^2.4",
            "doctrine/doctrine-migrations-bundle": "^3.2",
            "doctrine/orm": "^2.10",
            "phpdocumentor/reflection-docblock": "^5.3",
            "sensio/framework-extra-bundle": "^5.1",
            "symfony/asset": "4.4.*",
            "symfony/console": "4.4.*",
            "symfony/dotenv": "4.4.*",
            "symfony/expression-language": "4.4.*",
            "symfony/flex": "^1.3.1",
            "symfony/form": "4.4.*",
            "symfony/framework-bundle": "4.4.*",
            "symfony/http-client": "4.4.*",
            "symfony/intl": "4.4.*",
            "symfony/mailer": "4.4.*",
            "symfony/monolog-bundle": "^3.1",
            "symfony/process": "4.4.*",
            "symfony/property-access": "4.4.*",
            "symfony/property-info": "4.4.*",
            "symfony/proxy-manager-bridge": "4.4.*",
            "symfony/security-bundle": "4.4.*",
            "symfony/serializer": "4.4.*",
            "symfony/translation": "4.4.*",
            "symfony/twig-bundle": "4.4.*",
            "symfony/validator": "4.4.*",
            "symfony/web-link": "4.4.*",
            "symfony/yaml": "4.4.*",
            "twig/extra-bundle": "^2.12|^3.0",
            "twig/twig": "^2.12|^3.0"
        },
        "require-dev": {
            "phpunit/phpunit": "^9.5",
            "symfony/browser-kit": "4.4.*",
            "symfony/css-selector": "4.4.*",
            "symfony/debug-bundle": "4.4.*",
            "symfony/maker-bundle": "^1.0",
            "symfony/phpunit-bridge": "^5.3",
            "symfony/stopwatch": "4.4.*",
            "symfony/web-profiler-bundle": "4.4.*"
        },
        "config": {
            "preferred-install": {
                "*": "dist"
            },
            "sort-packages": true
        },
        "autoload": {
            "psr-4": {
                "App\\": "src/"
            }
        },
        "autoload-dev": {
            "psr-4": {
                "App\\Tests\\": "tests/"
            }
        },
        "replace": {
            "paragonie/random_compat": "2.*",
            "symfony/polyfill-ctype": "*",
            "symfony/polyfill-iconv": "*",
            "symfony/polyfill-php71": "*",
            "symfony/polyfill-php70": "*",
            "symfony/polyfill-php56": "*"
        },
        "scripts": {
            "auto-scripts": {
                "cache:clear": "symfony-cmd",
                "assets:install %PUBLIC_DIR%": "symfony-cmd"
            },
            "post-install-cmd": [
                "@auto-scripts"
            ],
            "post-update-cmd": [
                "@auto-scripts"
            ]
        },
        "conflict": {
            "symfony/symfony": "*"
        },
        "extra": {
            "symfony": {
                "allow-contrib": false,
                "require": "4.4.*"
            }
        }
    }
    ```
    
- docker-compose.override.yml
    
    ```
    version: '3'
    
    services:
    ###> symfony/mailer ###
    mailer:
        image: schickling/mailcatcher
        ports: [1025, 1080]
    ###< symfony/mailer ###
    
    ###> doctrine/doctrine-bundle ###
    database:
        ports:
          - "5432"
    ###< doctrine/doctrine-bundle ###
    ```
    
- docker-compose.yml
    
    ```
    version: '3'
    
    services:
    ###> doctrine/doctrine-bundle ###
    database:
        image: postgres:${POSTGRES_VERSION:-13}-alpine
        environment:
          POSTGRES_DB: ${POSTGRES_DB:-app}
    # You should definitely change the password in production
    POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-ChangeMe}
          POSTGRES_USER: ${POSTGRES_USER:-symfony}
        volumes:
          - db-data:/var/lib/postgresql/data:rw
    # You may use a bind-mounted host directory instead, so that it is harder to accidentally remove the volume and lose all your data!
          # - ./docker/db/data:/var/lib/postgresql/data:rw
    ###< doctrine/doctrine-bundle ###
    
    volumes:
    ###> doctrine/doctrine-bundle ###
    db-data:
    ###< doctrine/doctrine-bundle ###
    ```
    
- phpunit.xml.dist
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    
    <!-- https://phpunit.readthedocs.io/en/latest/configuration.html -->
    <phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:noNamespaceSchemaLocation="vendor/phpunit/phpunit/phpunit.xsd"
             backupGlobals="false"
             colors="true"
             bootstrap="tests/bootstrap.php"
             convertDeprecationsToExceptions="false"
    >
        <php>
            <ini name="display_errors" value="1" />
            <ini name="error_reporting" value="-1" />
            <server name="APP_ENV" value="test" force="true" />
            <server name="SHELL_VERBOSITY" value="-1" />
            <server name="SYMFONY_PHPUNIT_REMOVE" value="" />
            <server name="SYMFONY_PHPUNIT_VERSION" value="9.5" />
        </php>
    
        <testsuites>
            <testsuite name="Project Test Suite">
                <directory>tests</directory>
            </testsuite>
        </testsuites>
    
        <coverage processUncoveredFiles="true">
            <include>
                <directory suffix=".php">src</directory>
            </include>
        </coverage>
    
        <listeners>
            <listener class="Symfony\Bridge\PhpUnit\SymfonyTestsListener" />
        </listeners>
    
        <!-- Run `composer require symfony/panther` before enabling this extension -->
        <!--
        <extensions>
            <extension class="Symfony\Component\Panther\ServerExtension" />
        </extensions>
        -->
    </phpunit>
    
    ```
    
- symfony.lock

## Добавление docker-compose nginx + php-fpm

- docker
    - nginx
        - Dockerfile
            
            ```php
            FROM nginx:latest
            ```
            
        - tt.conf
            
            ```php
            server {
                listen 80;
                root /var/www/public;
                index index.php;
                server_name tt.local;
                error_log  /var/log/nginx/error.log;
                access_log /var/log/nginx/access.log;
            
                location / {
                    index  index.php index.html index.htm;
                    if (!-e $request_filename) { rewrite ^/(.*)$ /index.php?q=$1 last;}
                }
            
                location ~ \.php$ {
            
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header Host $host;
                    proxy_http_version 1.1;
                    proxy_set_header Connection "";
                    fastcgi_pass   php-fpm:9000;
                    fastcgi_index  index.php;
                    include fastcgi_params;
                    fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
                    fastcgi_param DOCUMENT_ROOT $realpath_root;
                    fastcgi_read_timeout 600;
                }
            }
            ```
            
    - php-fpm
        - Dockerfile
            
            ```php
            FROM php:7.4-fpm
            
            RUN apt-get update && apt-get install -y \
                    curl \
                    wget \
                    git \
                    libfreetype6-dev \
                    libonig-dev \
                    libpq-dev \
                    libjpeg62-turbo-dev \
                    libmcrypt-dev \
                    libpng-dev \
                    libzip-dev \
                && pecl install mcrypt-1.0.3 \
                && docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip \
                && docker-php-ext-configure gd --with-freetype --with-jpeg \
                && docker-php-ext-install -j$(nproc) gd \
                && docker-php-ext-enable mcrypt \
            
            RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
            
            ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini
            WORKDIR /var/www
            
            CMD ["php-fpm"]
            ```
            
        - php.ini
            
            ```php
            memory_limit = 512M
            ```
            
    - docker-compose.yml
        
        ```php
        version: '3'
        
        services:
          nginx:
            build:
              context: ./docker/nginx
            container_name: nginx
            ports:
              - "80:80"
            volumes:
              - ./:/var/www
              - ./docker/nginx/tt.conf:/etc/nginx/conf.d/default.conf
            depends_on:
              - php-fpm
            networks:
              - tt
        
          php-fpm:
            container_name: php-fpm
            build:
              context: ./docker/php-fpm
            volumes:
              - ./:/var/www
            networks:
              - tt
        
        networks:
          tt:
        ```
        
    
    ## Добавление mySql контейнера
    
    - docker-compose.yml
        
        ```php
        version: '3'
        
        services:
          nginx:
            build:
              context: ./docker/nginx
            container_name: nginx
            ports:
              - "80:80"
            volumes:
              - ./:/var/www
              - ./docker/nginx/tt.conf:/etc/nginx/conf.d/default.conf
            depends_on:
              - php-fpm
            networks:
              - tt
        
          php-fpm:
            container_name: php-fpm
            build:
              context: ./docker/php-fpm
            volumes:
              - ./:/var/www
            depends_on:
              - db
            networks:
              - tt
        
          db:
            image: mysql
            container_name: mysql
            command: --default-authentication-plugin=mysql_native_password
            restart: always
            environment:
              MYSQL_ROOT_PASSWORD: dbpass
            networks:
              - tt
        
        networks:
          tt:
        ```
        

## Зачем .env файл

- Зачем .env файл
    
    Вместо того, чтобы определять переменные env в вашей оболочке или на веб-сервере, Symfony предоставляет удобный способ определить их внутри файла .env (с точкой в начале), расположенного в корне вашего проекта. Файл .env считывается и анализируется при каждом запросе, а его переменные env добавляются в переменные PHP $_ENV и $_SERVER. Любые существующие переменные env никогда не перезаписываются значениями, определенными в .env, поэтому вы можете комбинировать оба. Например, чтобы определить переменную окружения DATABASE_URL, показанную ранее в этой статье, вы можете добавить: # .env DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name" Этот файл должен быть зафиксирован в вашем репозитории и (в связи с этим) должен содержать только значения «по умолчанию», которые подходят для локальной разработки. Этот файл не должен содержать производственных значений. Помимо ваших собственных переменных окружения, этот файл .env также содержит переменные среды, определенные сторонними пакетами, установленными в вашем приложении (они автоматически добавляются Symfony Flex при установке пакетов).