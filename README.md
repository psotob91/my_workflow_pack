Mi flujo de trabajo para crear paquetes en R
================
Percy Soto-Becerra
4/30/23

- <a href="#mi-flujo-de-trabajo-para-crear-paquetes"
  id="toc-mi-flujo-de-trabajo-para-crear-paquetes"><span
  class="toc-section-number">1</span> Mi flujo de trabajo para crear
  paquetes</a>
  - <a href="#instalar-paquetes" id="toc-instalar-paquetes"><span
    class="toc-section-number">1.1</span> Instalar Paquetes</a>
  - <a href="#paso-1-crea-una-carpeta-nueva-para-el-paquete"
    id="toc-paso-1-crea-una-carpeta-nueva-para-el-paquete"><span
    class="toc-section-number">1.2</span> Paso 1: Crea una carpeta nueva
    para el paquete</a>
  - <a href="#paso-2-crea-las-funciones"
    id="toc-paso-2-crea-las-funciones"><span
    class="toc-section-number">1.3</span> Paso 2: Crea las funciones</a>
  - <a href="#paso-3-agrega-las-dependencias-necesarias"
    id="toc-paso-3-agrega-las-dependencias-necesarias"><span
    class="toc-section-number">1.4</span> Paso 3: Agrega las dependencias
    necesarias</a>
  - <a href="#paso-4-agrega-los-datos-necesarios."
    id="toc-paso-4-agrega-los-datos-necesarios."><span
    class="toc-section-number">1.5</span> Paso 4: Agrega los datos
    necesarios.</a>
  - <a href="#paso-5-crea-la-plantilla-de-documentación"
    id="toc-paso-5-crea-la-plantilla-de-documentación"><span
    class="toc-section-number">1.6</span> Paso 5: Crea la plantilla de
    documentación</a>
  - <a href="#paso-6-crea-un-archivo-readme"
    id="toc-paso-6-crea-un-archivo-readme"><span
    class="toc-section-number">1.7</span> Paso 6: Crea un archivo README</a>
  - <a href="#paso-7-inicializa-el-control-de-versiones"
    id="toc-paso-7-inicializa-el-control-de-versiones"><span
    class="toc-section-number">1.8</span> Paso 7: Inicializa el control de
    versiones</a>
  - <a href="#paso-crea-una-licencia-mit"
    id="toc-paso-crea-una-licencia-mit"><span
    class="toc-section-number">1.9</span> Paso: Crea una licencia MIT</a>
  - <a href="#paso-10-verifica-el-paquete"
    id="toc-paso-10-verifica-el-paquete"><span
    class="toc-section-number">1.10</span> Paso 10: Verifica el paquete</a>

# Mi flujo de trabajo para crear paquetes

Este repo contiene mi flujo de trabajo para crear paquetes en R, luego
de haber revisado literatura, tutoriales y consultado algunos detalles a
ChatGPT4. Este flujo será actualizado conforme aprenda más sobre
desarrollar paquetes.

## Instalar Paquetes

``` r
#install.packages(c("usethis", "devtools", "testthat", "here"))
library(usethis)
library(devtools)
library(testthat)
library(here)
```

## Paso 1: Crea una carpeta nueva para el paquete

Crea un directorio para paquete. Lo mejor es usar la función de
Rproject. La otra opción es usar la función `usethis::create_package()`.
Sin embargo, esta opción implica que debas cambiar de directorio raíz y
las cosas pueden ponerse algo complicadas.

``` r
usethis::create_package("swper")
```

## Paso 2: Crea las funciones

``` r
# Crear los scripts R
usethis::use_r("swper_verify")
usethis::use_r("swper_score")
```

Asegurate de crear las funciones.

## Paso 3: Agrega las dependencias necesarias

``` r
# Agregar las dependencias
usethis::use_package("dplyr")
usethis::use_package("tidyr")
usethis::use_package("purrr")
usethis::use_package("tibble")
usethis::use_package("rlang")
```

Solo si se quiere permitir usar tidy_eval utils se puede hacer esto:

``` r
usethis::use_tidy_eval()
```

Ver más aquí
[enlace](https://stackoverflow.com/questions/58026637/no-visible-global-function-definition-for)

## Paso 4: Agrega los datos necesarios.

``` r
# Agregar los datos
usethis::use_data("example_data")
```

## Paso 5: Crea la plantilla de documentación

``` r
# Agregar la plantilla de documentación
usethis::use_roxygen_md("swper_verify")
usethis::use_roxygen_md("swper_score")
```

Nos advierte que hay que correr `document()`:

``` r
devtools::document()
```

``` r
devtools::document(roclets = c("rd", "collate", "namespace"))
```

Borramso el archivo NAMESPACE para que se vuelva a generar:

``` r
unlink(NAMESPACE)
```

También podemos preguntar la version de los paquetes para poder usarlos
en la documentacion:

``` r
# obtener la versión de los paquetes instalados actualmente
library(dplyr)

pkg_info <- installed.packages() %>%
  as_tibble() %>%
  select(Package, Version)

# obtener la versión de los paquetes cargados actualmente
loaded_pkgs <- names(sessionInfo()$loadedOnly)

# filtrar los paquetes cargados actualmente
pkg_info2 <- pkg_info %>% 
  filter(Package %in% loaded_pkgs)
```

``` r
# Todos los paquetes instalados
pkg_info
```

``` r
# Todos los paquetes usados en la sesion
pkg_info2
```

## Paso 6: Crea un archivo README

``` r
# Agregar el README
usethis::use_readme_md()
```

## Paso 7: Inicializa el control de versiones

``` r
# Inicializar el control de versiones
usethis::use_git()
```

## Paso: Crea una licencia MIT

``` r
usethis::use_mit_license()
```

## Paso 10: Verifica el paquete

``` r
unlink("NAMESPACE")
```

``` r
devtools::document(roclets = c("rd", "collate", "namespace"))
```

``` r
devtools::check()
```
