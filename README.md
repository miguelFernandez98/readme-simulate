# readme-simulate
## Descripción

La API de Stock se utiliza para realizar inventarios de productos mediante el uso de códigos de barras. Internamente, establece conexión con dos entornos de base de datos. Utiliza **`SQL Lite`** con Prisma para almacenar datos de conexión, historial y usuarios, y utiliza **`SQL Server`** para la gestión de carga de inventario.

## Pasos para levantar este Backend

### Preinstalación

Antes de clonar el repositorio y instalar sus dependencias es importante que tengan Node.js instalado en su entorno de trabajo.

- Versión mínima requerida de Node.js para Nest.js `v18.0.0`
- Versión de Node.js utilizada en este proyecto `v20.12.1`

Si necesita cambiar entre versiones de node.js puede usar [NVM](https://github.com/nvm-sh/nvm) para otros sistemas.

Para windows usar la versión: [NVM-WINDOWS](https://github.com/coreybutler/nvm-windows).
Recuerda que si vas a instalar nvm no tener una versión de node.js previamente instalada.

#### Instalacion de NPM

1. Instalar la versión de node

```bash
nvm install 20.12.1
```

2. Elegir la versión de node con nvm

```bash
nvm use 20.12.1
```

3. Verificar la instalación:
   Después de la instalación, puedes verificar si npm está instalado correctamente ejecutando el siguiente comando en la terminal:

```bash
npm --version
```

Para poder usar los comandos NPM o PNPM debera de configurar las variables de entorno en su sistema.

### Instalación

1. Clonar el repositorio
2. Instalar las dependencias con `pnpm` o `npm install`

```bash
# Use pnpm
$ pnpm

# Use npm
$ npm install
```

3. Configurar el archivo .env El archivo `.env` se encuentra en la raíz del proyecto y contiene varias variables de entorno necesarias para la configuración de la aplicación. Debe configurarse previamente antes de levantar el proyecto estas variables. Si no se agrega un `APIKEY`, el backend no se iniciará.

```bash
OMNI-STOCK-BACK
├── dist
├── node_modules
├── src
│   ├── app.module.ts
│   ├── main.ts
├── tests
├── **.env**                # Deberá de ser configurado.
├── package.json
├── README.md
└── tsconfig.json
```

Nota: Si el archivo no existe, crearlo.

#### Esta sería la estructura básica del archivo.

- Es crucial establecer el valor de **IP_HOST**, ya que representa la dirección IP donde se aloja el servidor. Este valor será utilizado para la generación del código QR.

- Debe de configurar previamente antes de levantar el proyecto estas variables.
  Si no se agrega un `APIKEY` el back-end no se iniciara, se recomienda que esta APIKEY sea diferente por cada sucursal.

- Además, se debe generar una `SECRET_KEY` al igual que la `APIKEY`. El back-end no se iniciará sin estas claves. La `SECRET_KEY` se utilizará para encriptar los parámetros de conexión en los códigos QR y debe ser la misma tanto en el back-end como en el front-end para garantizar la correcta desencriptación y validación de los datos.

| **Variable**                        | **Descripción**                                                                                      |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **EMAIL_HOST**                      | El host del servidor de correo electrónico (por ejemplo, `"smtp.example.com"`).                      |
| **EMAIL_PORT**                      | El puerto utilizado para la conexión al servidor (por ejemplo, `587` para TLS o `465` para SSL).     |
| **EMAIL_USER**                      | El nombre de usuario o dirección de correo electrónico utilizado para autenticarse en el servidor.   |
| **EMAIL_PASS**                      | La contraseña asociada al usuario o dirección de correo electrónico.                                 |
| **EMAIL_SECURE**                    | Indica si la conexión debe ser segura (por ejemplo, `"true"` para SSL o `"false"` para sin cifrado). |
| **LOGO_PDF**                        | La ruta o URL del archivo del logotipo PDF que deseas utilizar en tu aplicación.                     |
| **WHATSAPP_SERVER**                 | La URL del servidor que se encargará de enviar el mensaje de WhatsApp.                               |
| **WHATSAPP_APIKEY**                 | ApiKey que permite hacer las solicitudes.                                                            |
| **WHATSAPP_INSTANCE**               | La instancia de WhatsApp desde donde saldrá el mensaje.                                              |
| **ALERT_CRITICAL_PHONE_NUMBERS**    | Números de teléfonos donde se enviarán los mensajes de reportes de fallas del backend.               |
| **ALERT_CRITICAL_EMAIL_RECIPIENTS** | Correos donde se enviarán los mensajes de reportes de fallas del backend.                            |
| **ALERT_CRITICAL_ERROR_THRESHOLD**  | Cantidad mínima de errores que se tomará en cuenta para el envío del reporte.                        |
| **LOG_CLEANUP_INTERVAL**            | Intervalo en el cual se eliminaran los logs, por defecto es (EVERY_WEEKEND = "0 0 \* \* 6,0").       |

```bash
IP_HOST=0.0.0.0
HOST=http://0.0.0.0:<Puerto>/api
PORT=<Puerto>

JWT_SECRET=
ADMIN_SECRET=

LOGIN_EMAIL=
LOGIN_PASSWORD=

APIKEY=

SECRET_KEY=

EMAIL_HOST=
EMAIL_PORT=
EMAIL_USER=
EMAIL_PASS=
EMAIL_SECURE=

LOGO_PDF=

# WHATSAPP
WHATSAPP_SERVER=
WHATSAPP_APIKEY=
WHATSAPP_INSTANCE=

ALERT_CRITICAL_PHONE_NUMBERS=+58 0000000
ALERT_CRITICAL_EMAIL_RECIPIENTS=support@example.com
ALERT_CRITICAL_ERROR_THRESHOLD=10

LOG_CLEANUP_INTERVAL=0 0 * * 6,0
```

4. Ejecutar las migraciones de Prisma con `npx prisma migrate dev`. Este comando aplica las migraciones a tu base de datos.

```bash
$ npx prisma migrate dev
$ pnpm dlx prisma migrate dev
```

5. Usar el comando de prisma migrate reset permitira borrar la base de datos y cargara los valores por default que se agregaron previamente en las variables de entorno.

```bash
$ npx prisma migrate reset
$ pnpm dlx prisma migrate reset
```

Nota: Si desea hacer solo el `seed` de prisma usar el comando:

```bash
$ npx prisma db seed
```

### Ejecutando la aplicación

1. Correr el servidor con `pnpm run start:dev` o `npm run start:dev`

```bash
# Use pnpm
$ pnpm run start:dev

# Use npm
$ npm run start:dev
```

## Generar reportes desde back-end

### Instrucciones para Crear un Nuevo Reporte

Si deseas crear un nuevo reporte en el backend, sigue los pasos a continuación:

1. **Nuevo Módulo:**
   Si el reporte pertenece a un **módulo nuevo**, por favor dirígete a la **[Sección 1](#seccion-1-nuevo-modulo)**.

2. **Módulo Existente:**
   Si el reporte es para un **módulo ya existente**, continúa con la **[Sección 2](#seccion-2-modulo-existente)**.

### Sección 1: Nuevo Módulo

Para crear un **nuevo módulo de reportes** en el backend, es necesario seguir algunos pasos clave. A continuación, se muestra un diseño de la estructura básica de cómo debe estar organizada la carpeta del módulo de reportes en el proyecto.

```bash
OMNI-STOCK-BACK
├── src
│   ├── /reports
│   │   ├── /controllers
│   │   ├── /dictionary
│   │   ├── /dto
│   │   ├── /interfaces
│   │   ├── /services
│   │   ├── /sql
│   │   ├── /template
│   │   ├── reports.module.ts
│   │
│   ├── app.module.ts
│   ├── main.ts
├── tests
├── .env
├── package.json
├── README.md
```

Cada carpeta tiene una función específica dentro del módulo de reportes:

- Controllers: Definen los endpoints para las operaciones del módulo.
- Dictionary: Contiene configuraciones o diccionarios específicos del módulo.
- DTO: Define los Data Transfer Objects que se utilizan para el transporte de datos.
- Interfaces: Define las interfaces que utiliza el módulo.
- Services: Aloja la lógica de negocio relacionada con los reportes.
- SQL: Contiene las consultas SQL necesarias.
- Template: Define las plantillas que se usarán para generar los reportes.

Primero, enfoquémonos en la carpeta `dictionary` y su estructura dentro del módulo de reportes. A continuación, se muestra cómo está organizada:

```bash
OMNI-STOCK-BACK
├── src
│ ├── /reports
│ │ ├── /dictionary
│ │ │ ├── audit-meta-data.dictionary.ts
│ │ │ ├── clients-meta-data.dictionary.ts
│ │ │ ├── index.ts
│ │ │ ├── inventory-meta-data.dictionary.ts
│ │ │ ├── reports-all-meta-data.dictionary.ts
│ │ │ ├── reports-meta-data.dictionary.ts
│ │ │ ├── reports.dictionary.ts
│ │ │ ├── sales-meta-data.dictionary.ts
│ │ │ ├── shopping-meta-data.dictionary.ts
│ │ ├── reports.module.ts
│ │
│ ├── app.module.ts
│ ├── main.ts
├── tests
├── .env
├── package.json
├── README.md

```

#### Descripción de los archivos clave:

**Dictionary:** Esta carpeta contiene los metadatos y configuraciones esenciales para la creación de reportes en el módulo. Aquí se define la estructura de cada reporte, sus parámetros, y los servicios que se utilizan para obtener los datos. Los metadatos de los reportes especifican aspectos como el tipo de reporte, los parámetros que el usuario debe seleccionar, y las reglas de validación para los datos. Además, los metadatos hacen referencia a los servicios que ejecutan las consultas para generar los reportes. Por ejemplo:

- **audit-meta-data.dictionary.ts:** Contiene la definición de los metadatos específicos para los reportes relacionados con auditorías. Define la estructura, parámetros, y lógica que se utilizarán para generar este tipo de reportes.

- **clients-meta-data.dictionary.ts:** Define los metadatos para los reportes centrados en clientes, como el reporte de "Top Clientes", donde se especifican los parámetros requeridos, como el año, y las consultas relacionadas con la actividad de los clientes.

- **inventory-meta-data.dictionary.ts:** Almacena los metadatos para los reportes relacionados con el inventario. Aquí se detalla cómo deben presentarse los datos de inventario y qué parámetros se deben pasar para generar los reportes.

- **reports-all-meta-data.dictionary.ts:** Este archivo agrupa todos los metadatos de los distintos tipos de reportes, permitiendo una vista unificada y centralizada de todos los reportes disponibles en el sistema.

- **reports-meta-data.dictionary.ts:** Define los metadatos individuales de cada reporte disponible en el sistema. Este archivo es clave para estructurar la información de cada reporte y asociarla con los servicios correspondientes.

- **reports.dictionary.ts:** Implementa un diccionario dinámico que mapea los distintos reportes con su lógica de ejecución. Es aquí donde se integran las reglas de validación de los parámetros y las consultas a los servicios para generar el reporte. Los reportes pueden estar basados en ventas, inventarios, auditorías, entre otros.

- **sales-meta-data.dictionary.ts:** Contiene los metadatos para los reportes de ventas. Define cómo se deben generar los reportes de ventas diarias por sucursal, los métodos de pago, y las auditorías de turnos de ventas, entre otros.

- **shopping-meta-data.dictionary.ts:** Define los metadatos relacionados con los reportes de compras. Permite gestionar cómo se muestran y consultan los datos de compras a través de los servicios específicos.

#### Flujo de Trabajo:

Cuando se crea un nuevo reporte, primero se definen los `metadatos` en los archivos correspondientes dentro de la carpeta dictionary. Estos metadatos incluyen:

- El tipo de reporte: Por ejemplo, ventas, clientes, inventario, auditoría.
- Parámetros de entrada: Los valores que el usuario debe seleccionar para generar el reporte (como año, sucursal, fechas).
- Lógica de validación: Se valida que los parámetros ingresados por el usuario sean correctos antes de proceder con la generación del reporte.
- Conexión con servicios: Cada reporte está asociado a un servicio que ejecuta las consultas y devuelve los datos necesarios.

**Primeros pasos para crear un nuevo módulo de Reportes:**

Para crear un nuevo módulo de reportes en el backend, sigue los pasos a continuación:

1. Definir el nuevo módulo en `reports-meta-data.dictionary.ts`:

   Comencemos creando el módulo de **Clientes**. Para ello, es necesario seguir estos pasos:

   - Crear un nuevo archivo que contendrá los metadatos específicos del módulo. En este caso, llamaremos al archivo `clients-meta-data.dictionary.ts`.
   - Luego, debemos asegurarnos de importar este nuevo archivo dentro de `reports-meta-data.dictionary.ts` para incluirlo en la lista de módulos de reportes disponibles. Esto asegura que el nuevo módulo sea reconocido por el sistema y se integre adecuadamente en el flujo de generación de reportes.

2. Ejemplo de Implementación:

   - **Paso 1:** Crear el archivo `clients-meta-data.dictionary.ts` que contendrá los metadatos y configuraciones específicas del módulo de `Clientes`:

   ```bash
     // src/reports/dictionary/clients-meta-data.dictionary.ts
     import { typeSelect } from 'src/common/utils';
     import { ReportMetadataItem, ServiceNames } from '../interfaces';

     export const ClientsMetadata: ReportMetadataItem[] = [
       {
         serviceName: ServiceNames.TOPCLIENTS,
         params: [
           typeSelect('Año', [
             { value: '2020', label: '2020' },
             { value: '2021', label: '2021' },
             { value: '2022', label: '2022' },
             { value: '2023', label: '2023' },
             { value: '2024', label: '2024' },
             { value: '2025', label: '2025' },
           ]),
         ],
         name: 'Top Clientes',
         icon: 'sym_r_group',
       },
     ];

   ```

   Este archivo define un reporte para el módulo de `Clientes`, en este caso, el reporte **"Top Clientes"**, donde se selecciona el año como parámetro.
   Un paso previo a esto es la creación de `ServiceNames.TOPCLIENTS`. El `ServiceNames` es un `enum` que se encarga de mapear los diferentes nombres de servicios de reportes, permitiendo que el sistema identifique y encuentre el reporte adecuado de manera eficiente.

   - **Paso 2:** Importar el nuevo archivo en `reports-meta-data.dictionary.ts` para que el sistema lo reconozca:

   ```bash
     // src/reports/dictionary/reports-meta-data.dictionary.ts
     import { ReportsMetadataInterface } from '../interfaces';
     import { CountingGuideService } from 'src/inventory/services';
     import {
       AuditMetaData,
       InventoryMetaData,
       ShoppingMetaData,
       SalesMetadata,
       ClientsMetadata,
     } from '.';
     export const ReportsMetadata = async (
       countingGuideService?: CountingGuideService,
     ): Promise<ReportsMetadataInterface> => {
       return {
         VENTAS: SalesMetadata,
         COMPRAS: ShoppingMetaData,
         INVENTARIO: await InventoryMetaData(countingGuideService),
         CLIENTES: ClientsMetadata, # Se define el documento ClientesMetaData
         AUDITORIA: AuditMetaData,
       };
     };
   ```

   En este archivo, estamos incluyendo el módulo Clientes mediante la importación de ClientsMetadata. Esto lo asocia al conjunto global de módulos de reportes que el sistema puede gestionar.

   - **Paso 3:** Definir el nuevo modulo dentro de `reports-all-modules.dictionary.ts`:

   ```bash
     // src/reports/dictionary/reports-all-modules.dictionary.ts
     import { Modules } from '../interfaces';
     import { ModuleMetadata } from '../interfaces/module-meta-data.interface';
     export const ModulesAllData: ModuleMetadata[] = [
       {
         moduleName: Modules.VENTAS,
         name: 'ventas',
         icon: 'sym_r_attach_money',
       },
       {
         moduleName: Modules.COMPRAS,
         name: 'compras',
         icon: 'sym_r_shopping_cart',
       },
       {
         moduleName: Modules.INVENTARIO,
         name: 'inventario',
         icon: 'sym_r_box',
       },
       {
         moduleName: Modules.AUDITORIA,
         name: 'auditoría',
         icon: 'sym_r_policy',
       },
       {
         moduleName: Modules.CLIENTES, # Nuevo modulo Clientes
         name: 'clientes',
         icon: 'sym_r_group',
       },
     ]
   ```

   Esto nos permitirá incluir el nuevo módulo en el endpoint de módulos, asegurando su visibilidad y accesibilidad dentro del sistema. Eso lo veremos mas adelante en esta documentación.

   - **Paso 4:** Agregar la Lógica Correspondiente en dentro de `reports.dictionary.ts`:

     El archivo `reports.directory.ts` es el que se encargara de mapear los distintos reportes con su lógica de ejecución. Es aquí donde se integran las reglas de validación de los parámetros y las consultas a los servicios.

     Es importante definir el `Dto` con el cual se trabajara, en este caso `TypeSelectParamsDto`

   ```bash
    // src/reports/dictionary/reports.dictionary.ts
    TOPCLIENTS: async (uuid: string, queryParams: TypeSelectParamsDto): Promise<string> => {
      try {
        // Validar los parámetros de la consulta
        await validateQueryParams(queryParams.queryParams, TypeSelectParamsDto);
        // Obtener los parámetros necesarios
        const year = queryParams.queryParams.year as string;
        // Llamar al servicio correspondiente
        return await services.clientsService.getTopClientsByYear(uuid, year);
      } catch (error) {
        throw error; // Manejar el error adecuadamente
      }
    },
   ```

   Es importante definir o importar el servicio de clientes, en este caso `clientsService` y el método `getTopClientsByYear` que se usan dentro de `TOPCLIENTS`

3. Definición de los `Servicios`

   Ya definido todo en la carpeta `directory`, enfoquémonos en la carpeta `services` y su estructura dentro del módulo de reportes. A continuación, se muestra cómo está organizada:

   ```bash
    OMNI-STOCK-BACK
    ├── src
    │ ├── /reports
    │ │ ├── /services
    │ │ │ ├── clients.service.ts
    │ │ │ ├── company-data.service.ts
    │ │ │ ├── index.ts
    │ │ │ ├── inventory.service.ts
    │ │ │ ├── reports.service.ts
    │ │ │ ├── sales.service.ts
    │ │ │ ├── serviceRequest.service.ts
    │ │ │ ├── services.service.ts
    │ │ ├── reports.module.ts
    │ │
    │ ├── app.module.ts
    │ ├── main.ts
    ├── tests
    ├── .env
    ├── package.json
    ├── README.md
   ```

#### Descripción de los Archivos Clave en services:

- **Services:** Esta carpeta contiene la lógica de negocio y las funciones que interactúan con las bases de datos y otros servicios externos para obtener datos relevantes para la generación de reportes. Cada archivo dentro de esta carpeta se encarga de un aspecto específico de los reportes, proporcionando métodos que pueden ser utilizados en los diccionarios de reportes.

  - **clients.service.ts:** Este archivo define el servicio relacionado con la gestión de clientes. Contiene métodos que permiten realizar operaciones como obtener información de clientes, generar reportes específicos sobre ellos (por ejemplo, el reporte de "Top Clientes"), y manejar la lógica de negocio asociada.

  - **company-data.service.ts:** Este servicio maneja la información de la empresa, proporcionando métodos que permiten acceder a datos específicos de la compañía. Puede incluir reportes que integren información a nivel de la empresa, como desempeño general y métricas financieras.

  - **index.ts:** Este archivo es comúnmente usado para exportar los servicios disponibles dentro de la carpeta. Facilita la importación de múltiples servicios desde un único punto, simplificando la gestión de dependencias en otros archivos.

  - **inventory.service.ts:** Contiene la lógica necesaria para gestionar el inventario. Este servicio permite obtener reportes relacionados con el inventario, como niveles de stock, movimientos de inventario y auditorías de existencias.

  - **reports.service.ts:** Este servicio es fundamental ya que coordina la generación de reportes. Contiene métodos que pueden combinar datos de diferentes servicios para generar reportes compuestos, como reportes de ventas que también incluyen información de clientes y productos.

  - **sales.service.ts:** Define la lógica para manejar reportes de ventas. Incluye métodos para acceder a información sobre transacciones, métodos de pago, ventas por sucursal, y auditorías de ventas.

  - **serviceRequest.service.ts:** Este archivo gestiona las solicitudes a servicios externos necesarios para la generación de reportes.

  - **services.service.ts:** Este archivo actúa como un punto centralizado para otros servicios que no encajan en las categorías anteriores, permitiendo una mayor flexibilidad en la estructuración de la lógica de negocio.

  **A continuación se define los pasos para la creación de un nuevo servicio, en este caso `clients.service.ts`**

  - **Paso 1:** Crear el archivo `clients.service.ts` que contendrá los logica de negocio para obtener la infomación correspondiente de los `Clientes`:

  ```bash
    // src/reports/services/clients.service.ts
    import {
      BadRequestException,
      Inject,
      Injectable,
      Logger,
    } from '@nestjs/common';
    import { ConfigType } from '@nestjs/config';

    import {
      DatabaseService,
      DatabaseUtilityService,
      OrmService,
    } from 'src/database/services';
    import config from 'src/settings/env-config/env-config';
    import { savePdfToFolder } from 'src/common/utils';
    import { topClients } from '../sql';
    import { CompanyDataService } from './company-data.service';
    import { TopClientsPDF } from '../template';

    @Injectable()
    export class ClientsService {
      private readonly logger = new Logger(ClientsService.name);

      constructor(
        @Inject(config.KEY)
        private readonly configService: ConfigType<typeof config>,
        private readonly ormService: OrmService,
        private readonly dbService: DatabaseService,
        private readonly databaseUtilityService: DatabaseUtilityService,
        private readonly companyDataService: CompanyDataService,
      ) {}

      async getTopClientsByYear(uuid: string, year: string) {
        const cia = this.dbService.sqlConfig.dbos;

        const query = topClients(cia, year);

        const params = this.databaseUtilityService.createParams({
          cia: cia ? cia : 'NULL',
          year,
        });

        try {
          const dataCompany = await this.companyDataService.getDataCompany();

          const topClients = await this.ormService.multiParamsQuery<any[]>(
            query,
            params,
          );

          if (!topClients?.length) {
            throw new BadRequestException(
              'No se encontraron datos para el reporte.',
            );
          }

          const docDefinition = await TopClientsPDF(
            dataCompany,
            topClients,
            this.configService.logoPdf,
          );

          const name = await savePdfToFolder(uuid, docDefinition, this.logger);

          return name;
        } catch (error) {
          throw error;
        }
      }
    }
  ```

  Este servicio realiza la consulta de los principales clientes del año especificado, generando un reporte en formato PDF que se guarda en una carpeta del servidor. Es necesario crear el `template` del PDF correspondiente en la carpeta template, y las consultas **SQL** utilizadas en este servicio deben definirse dentro de la carpeta `sql`. Más adelante en esta documentación se explicará cómo definir tanto el template del PDF como las consultas SQL correspondientes.

  - **Paso 2:** Inyectamos el servicio `clients.service.ts` dentro del servicio `services.service.ts`:

  ```bash
    // src/reports/services/services.service.ts
    import { SaleService, InventoryService, ClientsService } from '.';

    export class Services {
      constructor(
        public readonly ventasService: SaleService,
        public readonly inventoryService: InventoryService,
        public readonly clientsService: ClientsService,
        // Agrega aquí todos los otros servicios que necesitas
      ) {}
    }
  ```

  - **Paso 3:** Inyectamos el servicio `clients.service.ts` en el constructor de `reports.service.ts`:

  ```bash
    // src/reports/services/reports.service.ts
    @Injectable()
    export class ReportsService {
      private readonly logger = new Logger(ReportsService.name);
      private time: number = 5 * 60 * 1000;

      private readonly reportsDictionary: Record<
        string,
        (uuid: string, queryParams: QueryParamsDto) => Promise<string>
      >;

      private serviceRequests: Record<string, ServiceRequest<QueryParamsDto>> = {};

      constructor(
        private readonly profileService: ProfileService,
        private readonly mailService: MailService,
        private readonly whatsAppService: WhatsAppService,
        private readonly saleService: SaleService,
        private readonly inventoryService: InventoryService,
        private readonly countingGuideService: CountingGuideService,
        private readonly clientService: ClientsService, # Servicio clienteService
      ) {
        this.reportsDictionary = createReportsDictionary(
          new Services(this.saleService, this.inventoryService, this.clientService), # Es importante definice aqui tambien
        );
      }
    }
  ```

  La inyección de `ClientsService` en `ReportsService` permite usar la lógica de clientes en la generación de reportes, asegurando modularidad y facilitando la ejecución dinámica de cada reporte según su servicio asociado.

4. Generación de las consultas **SQL**

   Enfoquémonos en la carpeta `sql` y su estructura dentro del módulo de reportes. A continuación, se muestra cómo está organizada:

   ```bash
    OMNI-STOCK-BACK
    ├── src
    │ ├── /reports
    │ │ ├── /sql
    │ │ │ ├── index.ts
    │ │ │ ├── sales-branch.per-day.ts
    │ │ │ ├── top-clients.sql.ts
    │ │ │ ├── total-counter-guide.sql.ts
    │ │ ├── reports.module.ts
    │ │
    │ ├── app.module.ts
    │ ├── main.ts
    ├── tests
    ├── .env
    ├── package.json
    ├── README.md
   ```

#### Descripción de los Archivos Clave en /sql:

- **SQL:** Esta carpeta contiene las consultas SQL necesarias para la generación de reportes. Cada archivo dentro de esta carpeta está dedicado a una consultespecífica, que será utilizada por los servicios correspondientes para recuperar los datos desde la base de datos.

  - **index.ts:** Este archivo centraliza y exporta todas las consultas SQL disponibles en la carpeta. Facilita la importación de las consultas desde un úni lugar en otros archivos y servicios, simplificando la gestión de dependencias.

  - **sales-branch.per-day.ts:** Este archivo contiene la consulta SQL específica que permite obtener información de las ventas por sucursal en un d determinado. Este tipo de consulta es utilizado para generar reportes diarios de ventas.

  - **top-clients.sql.ts:** Define la consulta SQL que se utiliza para obtener el reporte de los "Top Clientes". Este archivo incluye la lógica para seleccionar los principales clientes de acuerdo a los criterios definidos (como ventas o volumen).

  - **total-counter-guide.sql.ts:** Contiene la consulta que calcula el total de las guías de conteo. Este tipo de reporte puede ser utilizado para auditorí y control de inventarios.

  **A continuación se define los pasos para la creación de una nueva consulta SQL, en este caso `top-clients.sql.ts`**

  - **Paso 1:** Crear el archivo `top-clients.sql.ts` que se usara para obtener la infomación correspondiente de los `Clientes`:

  ```bash
    // src/reports/sql/top-clients.sql.ts
    export const topClients = (OScia: string, year: string) => `
    `;
  ```

  Recuerda definir la consulta sql que se usara para obtener los datos correspondientes

  - **Paso 2:** importar esta nueva consulta dentro del **servicio** `clients.service.ts`:

  ```bash
    // src/reports/services/clients.service.ts
    async getTopClientsByYear(uuid: string, year: string) {
      const cia = this.dbService.sqlConfig.dbos;

      const query = topClients(cia, year); # definimos la consulta sql

      const params = this.databaseUtilityService.createParams({
        cia: cia ? cia : 'NULL',
        year,
      });
    }
  ```

  Como se explicó previamente en detalle en la sección del servicio `clients.service.ts`,
  aquí se utiliza la consulta SQL topClients para extraer los datos necesarios.Esta consulta, definida en la carpeta /sql,
  permite obtener información sobre los clientes más importantes del año indicado.

5. Definición del **TEMPLATE**

   Enfoquémonos en la carpeta `template` y su estructura dentro del módulo de reportes. A continuación, se muestra cómo está organizada:

   ```bash
    OMNI-STOCK-BACK
    ├── src
    │ ├── /reports
    │ │ ├── /template
    │ │ │ ├── index.ts
    │ │ │ ├── inventory-counter-guide.template.ts
    │ │ │ ├── sales-branch-day.template.ts
    │ │ │ ├── sales-payments-instruments.template.ts
    │ │ │ ├── sales-turn-audit.template.ts
    │ │ │ ├── top-clients.template.ts
    │ │ ├── reports.module.ts
    │ │
    │ ├── app.module.ts
    │ ├── main.ts
    ├── tests
    ├── .env
    ├── package.json
    ├── README.md
   ```

**Descripción de los Archivos Clave en /template:**

- **TEMPLATE:** Esta carpeta contiene los templates que definen el formato y diseño de los reportes generados en PDF. Cada archivo dentro de esta carpeta est dedicado a un tipo de reporte específico, proporcionando el layout y estructura de cada uno de ellos.

  - **index.ts:** Este archivo centraliza y exporta todos los templates disponibles dentro de la carpeta, facilitando la importación desde un único lugar para su us en otros archivos y servicios.

  - **inventory-counter-guide.template.ts:** Contiene el diseño del template para los reportes de guías de conteo de inventario. Define el formato visual y la estructura que se utilizará al generar este tipo de reporte.

  - **sales-branch-day.template.ts:** Define el layout del reporte de ventas diarias por sucursal, que es utilizado al generar un PDF con este tipo de reporte.

  - **sales-payments-instruments.template.ts:** Este archivo contiene el template para los reportes que detallan los instrumentos de pago utilizados en ventas. Incluye el formato para presentar esta información de manera clara.

  - **sales-turn-audit.template.ts:** Proporciona el diseño del template para auditorías de turnos de ventas, que se emplea al generar reportes relacionados con las auditorías de transacciones en los turnos de trabajo.

  - **top-clients.template.ts:** Define el template utilizado para generar el reporte de los "Top Clientes". Este template establece cómo se mostrarán los principales clientes dentro del PDF.

  **A continuación se define los pasos para la creación de un nuevo template, en este caso `top-clients.template.ts`**

  - **Paso 1:** Crear el archivo `top-clients.template.ts` que contendrá la estructura de como se mostrara el reporte PDF:

  Se debe recordar que la generación de los template funciona con la dependencia `pdfmaker`

  ```bash
    // src/reports/template/top-clients.template.ts
    export const TopClientsPDF = async (
      dataCompany: BranchData,
      data: any[],
      imgName: string,
    ): Promise<TDocumentDefinitions> => {
        return {
          footer: function () {
            const today = new Date();
            const formattedDate = today.toLocaleDateString();

            return {
              margin: [40, 0],
              columns: [
                {
                  text: `Fecha de generación: ${formattedDate}`,
                  alignment: 'right',
                  fontSize: 10,
                },
              ],
            };
          },
          content: []
      }
    }
  ```

  Igualemnte como con la consulta `sql` de clientes, el `template` tambien se debe de importar dentro de servicio `clients.service.ts`

  - **Paso 2:** importar el template dentro del **servicio** `clients.service.ts`:

  ```bash
    // src/reports/services/clients.service.ts
    async getTopClientsByYear(uuid: string, year: string) {
    try {
      const docDefinition = await TopClientsPDF(
        dataCompany,
        topClients,
        this.configService.logoPdf,
      );
      const name = await savePdfToFolder(uuid, docDefinition, this.logger);
      return name;
    } catch (error) {
      throw error;
    }
  ```

6. Gestión de permisos y asignación de perfiles para módulos y reportes generados

Cada vez que se crea un nuevo módulo en el sistema, es fundamental asignar los permisos y perfiles adecuados para que los usuarios puedan acceder y ejecutar las funcionalidades correspondientes. Esto implica crear una migración manual donde se registren los detalles del módulo, los endpoints asociados y los permisos específicos.

**- Pasos para la Creación de Permisos y Perfiles:**

1. Definir el Nuevo Módulo
   Crear una entrada en la tabla Module para registrar el nuevo módulo. Esto incluye el nombre del módulo y sus marcas de tiempo.

2. Registrar los Endpoints y Permisos
   Cada ruta del nuevo módulo debe tener un permiso asociado, que se registra en la tabla Features. Es importante especificar el moduleId, el método HTTP, el nombre del permiso, y la ruta del endpoint.

3. Creación de la Migración Manual

   Sigue los siguientes pasos para crear la migración manual:

   1. Creación de la migración manual
      Ejecuta el siguiente comando para crear la migración:

   ```bash
   npx prisma migrate dev --create-only --name <NOMBRE_DE_TU_MIGRACIÓN>
   ```

   2. Definición de los Permisos y Features
      En la misma migración, define los permisos para el nuevo módulo. Aquí tienes un ejemplo para un módulo llamado `CLIENTES`

   ```bash
   INSERT INTO "Features" ("moduleId", "method", "name", "path", "createdAt", "updatedAt") VALUES

   -- Permisos Globales para el módulo Clientes
   (4, 'all', 'Todos los clientes', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);

   -- Permisos específicos para endpoints en el módulo Clientes
   (4, 'GET', 'Obtener Reportes por Módulo clientes', '/api/reports/module/CLIENTES', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
   (4, 'POST', 'Generar Reporte clientes', '/api/reports/CLIENTES/:serviceName/generate', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);
   ```

   Se define el `moduleId` con valor **4**, dado que ese es el id que posee el modulo de reportes en la tabla `Module`.

   3. Ejecutar la Migración
      Una vez que hayas agregado los permisos y los detalles del nuevo módulo de Clientes, ejecuta la migración para aplicar los cambios:

   ```bash
   npx prisma migrate dev
   ```

   Con estos cambios, el módulo de Clientes queda registrado junto con los permisos necesarios para:

   - **Obtener reportes por módulo:** con un método GET para la ruta /api/reports/module/CLIENTES.
   - **Generar reportes de clientes:** con un método POST para la ruta /api/reports/CLIENTES/:serviceName/generate.
     Este proceso asegura que los reportes de clientes estén correctamente gestionados en términos de permisos y perfiles dentro del sistema. Asegúrate de que los demás módulos que gestionan reportes de otros departamentos (como inventario, compras, ventas) también estén alineados con este enfoque.

### Sección 2: Módulo Existente

Para generar un reporte desde el backend se necesita hacer ciertos pasos primero

1. Crear servicios para los módulos:

- Si aún no tienes servicios creados para los módulos, debes crearlos. Por ejemplo, `inventoryService.ts`.

- En tu archivo `service.service.ts`, asegúrate de agregar el nuevo servicio junto con los demás. Aquí tienes un ejemplo:

```bash
import { SaleService, InventoryService } from '.';

export class Services {
constructor(
public readonly ventasService: SaleService,
public readonly inventoryService: InventoryService,
// Agrega aquí todos los otros servicios que necesitas
) {}
}
```

2. Inyectar el servicio en el constructor de `reports.service.ts`:

- En tu archivo `reports.service.ts`, dentro del constructor, debes inyectar el servicio recién creado. Aquí está cómo hacerlo:

```bash
constructor(
    private readonly mailService: MailService,
    private readonly saleService: SaleService,
    private readonly inventoryService: InventoryService,
  ) {
    this.reportsDictionary = createReportsDictionary(
      new Services(this.saleService, this.inventoryService),
    );
  }
```

3. Enum de ServiceNames:

- En primer lugar, debes crear un nuevo nombre en el enum ServiceNames. Por ejemplo:

```bash
export enum ServiceNames {
  BRANCHSALESPERDAY = 'BRANCHSALESPERDAY',
  PAYMENTINSTRUMENTS = 'PAYMENTINSTRUMENTS',
  SALESTURNAUDIT = 'SALESTURNAUDIT',
  #Ejemplo agrega el nombre de tu servicio aquí
  NOMBRE_SERVICE = NOMBRE_SERVICE
  DEFAULT = 'DEFAULT',
}
```

- Asegúrate de generar un nombre descriptivo para tu servicio.
- Es importante generar el nombre del servicio.

2. Crear un método en el servicio:

Si estás agregando un nuevo método, crea uno en el servicio correspondiente o crea un nuevo servicio si es necesario.

Importa el nuevo método en el diccionario llamado `reports.dictionary.ts`. Por ejemplo:

```bash

    NOMBRE_SERVICE: async (
      uuid: string,
      queryParams: TuDto,
    ): Promise<string> => {
      await validateQueryParams(
        queryParams.queryParams,
        TuDto,
      );

      const date = queryParams.queryParams.date as string;
      #Recuerda llamar al servicio en el que estas trabajando
      return await services.ventasService.salesPaymentInstruments(uuid, date);
    },
```

3. Configuración para renderizar en el front:

- Para que el nuevo reporte se muestre en el front-end, debes agregarlo al diccionario `reports-meta-data`. O en el directorio donde se alla creado un ejemplo: `sales-meta-data.directory.ts`

- Aquí tienes un ejemplo de cómo definir los elementos de entrada (input) para el reporte:

```bash
    #Ejemplo de como llamar a los input 'Select', date y rango de fecha
    {
      serviceName: 'NOMBRE_SERVICE',
      params: [
        typeDate('Fecha'), # Input de tipo fecha
        ...typeDateRange('Fecha de inicio', 'Fecha final'), # Rango de fechas
        typeSelect('Opción', [
          { value: 'option1', label: 'Opción 1' },
          { value: 'option2', label: 'Opción 2' },
        ]), # Input de tipo selección (select)
      ],
      name: 'Multi Elements', # Nombre del reporte
      icon: 'icon2', # Icono asociado
    }
```

- Asegúrate de proporcionar etiquetas descriptivas para cada tipo de entrada (date, date range, select).
- El metodo typeDate('Fecha'), se le pasa un string el cual es el label que se renderizara en el front.
- El metodo ...typeDateRange('Fecha de inicio', 'Fecha final'), al igual que el anterior se debe pasar dos label.
- El metodo typeSelect recibe un label y un array con los valores que tendra el option.

4. Crear carpeta `assets/images`

- Se debe crear esta carpeta en la raiz del proyecto y cargar la imagen de la empresa se recomienda usar un tamaño de 120x120

- Luego llamarse en el reporte de esta forma:

```bash
 await convertImageTo64(nombreImagen); #llamar el nombre configurado en el archivo .env
```

Nota: En el .env se debe de agregar el nombre de la imagen a usar la ruta de la carpeta debe ser `assets/images`,

## Creación de Modulos y Features en el backend

Para realizar esto se deben de crear migraciones manual en el backend, siempre que se cree un nuevo endpoint se de debe incluir en un migración este mismo, donde inlucya lo siguiente:

Ejemplo:

- "moduleId": 1,
- "method": "GET",
- "name": "Obtener Reportes por Módulo compras",
- "path": "/api/reports/module/COMPRAS",

Se debe de incluir en la migración esos datos, el modulo al cual pertenece el metodo que ejecuta, una descripcion de ese metodo y path o ruta del endpoint.

1. Creacion de una migración manual

```bash
 npx prisma migrate dev --create-only --name <NOMBRE_DE_TU_MIGRACIÓN>
```

Ejemplo migración manual Module

```bash
INSERT INTO "Module" ("id", "name", "createdAt", "updatedAt")
VALUES (1, 'COMPRAS', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
       (2, 'VENTAS', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
       (3, 'AUDITORIA', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
       (4, 'REPORTES', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
       (5, 'INVENTARIO', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
       (6, 'SETTINGS', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
       (7, 'AUTH', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);
```

Ejemplo migración manual Features:

```bash
 INSERT INTO "Features" ("moduleId", "method", "name", "path", "createdAt", "updatedAt") VALUES
-- Permisos Globales por modulo
(7, 'all', 'Todo auth', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
(6, 'all', 'Todo settings', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
(5, 'all', 'Todo inventario', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
(4, 'all', 'Todo reportes', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
(3, 'all', 'Todo auditoria', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
(2, 'all', 'Todo ventas', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),
(1, 'all', 'Todo compras', 'all', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP),

-- Permisos por rutas
(7, 'POST', 'Crear Perfil', '/api/auth/profile', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);
```

Permisos y Features

- Cada módulo tiene métodos asociados con rutas y métodos.
- Los métodos con la palabra “all” en la ruta otorgan permisos a todos los Features del módulo.
- Los métodos “all” son acumulativos y pueden coexistir con otros permisos.

2. Ejecutar la migración

```bash
 npx prisma migrate dev
```

## Acceso a doc (Swagger)

Para acceder a la documentación de doc, deberás seguir los siguientes pasos:

1. Asegúrate de tener el servidor en ejecución. Para ello, puedes utilizar el comando `pnpm run start:dev`.
2. Observa la salida del comando anterior para determinar el puerto en el que se está ejecutando el servidor. Por lo general, será `3000`, pero podría variar dependiendo de la configuración de tu entorno.
3. Una vez que el servidor esté en funcionamiento y sepas el puerto, abre un navegador web y navega a la siguiente URL: `localhost:<puerto>/doc`. Reemplaza `<puerto>` con el número de puerto que observaste en el paso 2.

Ejemplo de ruta

```bash
localhost:3000/doc
```

Esto te llevará a la interfaz de doc, donde podrás ver y probar todos los endpoints disponibles en la API.

## Uso de Prisma Studio

Prisma Studio es una herramienta de visualización de bases de datos que te permite ver y modificar los datos de tu base de datos. Para abrir Prisma Studio, sigue estos pasos:

```bash
$ npx prisma studio
```

### ADMIN ADS VISOR - AdminAdsVisorController

Proporciona varios endpoints para gestionar los anuncios publicitarios del visor de Precios. Estos incluyen la obtención de anuncios, la carga de nuevos anuncios y la eliminación de anuncios existentes.

**Nota**
El logo de la empresa debe ser insertado manualmente en la ruta public/images/logo. Asegúrate de que el archivo del logo esté presente en esta ruta para que el endpoint GET /ads-visor/logo funcione correctamente.
Solo debe haber un solo archivo alli.

Este modulo necesita q las carpetas esten creadas: `public/images/logo` y `public/images/ads`
