# Sales Representatives — Ciudades, asociación de clientes y comisiones

Maqueta navegable y análisis técnico para incorporar **Representantes de Ventas** al
software interno y al portal de clientes de **Project Agenda**.

## Contenido

| Archivo | Qué es |
| --- | --- |
| `index.html` | Maqueta del **software interno**: clientes, representantes de ventas y leads. |
| `client-portal.html` | Maqueta del **portal de clientes** (página 2 del PDF): Main Board, Create Quote y documentos. |

Ambos son HTML autocontenidos, sin CDNs ni fuentes externas. Ábrelos con doble clic.

## Qué cubre la maqueta

**Software interno › Main Board**

- Réplica del tablero interno: **Bidding · Approval · Follow Up**, con conteo, total en COP
  y buscador por columna.
- **Filtros nuevos `Client` y `Sales Rep`** junto al de `Users`, del mismo tamaño y comportamiento.
  Al activarse, el control se tiñe de azul y muestra el nombre con una ✕ para limpiarlo.
  Cada uno lleva **buscador** y **selección múltiple** con casillas: varias opciones
  dentro de un filtro suman, y los tres filtros entre sí se acumulan. *Clear* en el
  pie del menú. Sin nada marcado equivale a todos.
- 21 cotizaciones de demostración repartidas a propósito (6 · 5 · 4 · 6 sin representante)
  para que el efecto del filtro se vea de inmediato.
- **Al pulsar una tarjeta** se abre la cotización con dos pestañas: *General
  Information* y *Costs*, con el nombre del proyecto de esa cotización.
- En *Costs*, la sección **Surcharges** admite varios recargos. Si la cotización
  tiene representante, se añade solo `Commission – <empresa representante>` al porcentaje del tramo
  que le toca por importe (valor sugerido). Todos los porcentajes son **editables**: el lápiz
  abre el diálogo *Commission* (Details y %, *Close* aplica y *Delete* lo quita), y al cambiarlos se
  recalculan los precios de venta y los totales:
  `sale = (coste + margen bruto) × (1 + Σ recargos)`.
- El tramo se decide sobre el **subtotal antes de recargos**, en USD (una cotización en
  pesos se convierte a la tasa de la maqueta, 4.020 COP por dólar). Medirlo sobre el total
  final sería circular: la comisión movería la cifra que decide la comisión.
- Las partidas de coste son ficticias, derivadas del importe de la tarjeta.
- *General Information* reproduce la vista real y añade un bloque pequeño **Sales Rep**
  con *Company* y *Contact*, editables. Cambiar la compañía rehace la comisión en
  *Costs* con el tramo del nuevo representante y recalcula los totales;
  dejarla vacía elimina esa comisión.
- **Solo se edita en `Bidding` y `Approval`.** En `Follow Up` toda la vista queda en
  solo lectura —representante, contacto, cliente y ubicación—, con los campos en gris,
  *Save* deshabilitado y un candado. La comprobación también está en el manejador, no
  solo en el marcado.

**Software interno › Clients**

- Segmentado `Clients | Sales Reps` en la cabecera del panel maestro. Mismo layout,
  misma lista, mismo detalle: no es un módulo aparte.
- **El representante es una empresa, no una persona.** Las personas viven en
  `contacts[]`, igual que en la ficha de cliente. El *Email* y el *Phone* de la
  ficha son de la empresa; cada contacto lleva el suyo.
- El bloque de contactos es **el mismo componente** en ambas secciones —buscador,
  alta en línea y borrado—. En la ficha de cliente era decorativo (ni el buscador
  ni el `+` tenían manejador); ahora funciona, y Sales Reps lo reutiliza en vez de
  crecer una segunda versión.
- El **cargo es opcional**: un contacto sin cargo muestra solo el nombre.
- Se pueden añadir contactos durante el alta, desde el propio formulario, o después
  desde la ficha.
- Ficha de representante con **varios estados** (su territorio), divisa, idioma, clientes asignados
  y los dos tramos de comisión.
- Asignación de clientes **desde el propio formulario de alta**, con chips y aviso
  de cuáles tienen *Directed Quotes* apagado. Al editar, la selección se sincroniza
  conservando la fecha original de las asignaciones que ya existían.
- Modal de asignación masiva para gestionarlo después.
- **Edición de clientes y representantes** desde el menú de tres puntos (⋮) de la
  cabecera de la ficha. El estado activo/inactivo sigue en el toggle de al lado.
- La *City* del cliente pasa a ser un desplegable del catálogo, no texto libre.
- El formulario de cliente (alta y edición) incluye **Address**, bajo el nombre; la
  dirección se muestra en la ficha cuando el cliente la tiene.
- Bloque **Sales Representatives** al final de la ficha de cliente, con los reps asignados.

**Software interno › Settings**

Las colecciones `cities`, `currencies` y `languages` siguen existiendo y alimentan
los desplegables — nada está escrito en el código. **La pantalla para administrarlas
se retiró de la maqueta por decisión de Natalia (8 de septiembre de 2026).**

- **Territorio por país y divisiones:** primero el **país** —están los 249 de la norma
  ISO 3166— y luego sus divisiones de primer nivel, con el nombre que tienen allí según
  ISO 3166-2: *state* en EE. UU. o México, *department* en Colombia, *province* en
  Argentina o Canadá, *region* en Chile o Perú, *autonomous community* en España...
  EE. UU. y Colombia usan las mismas listas que el resto de formularios, para que la
  sugerencia de Leads siga cuadrando. Los 49 territorios sin subdivisiones en la norma
  (Puerto Rico, Hong Kong...) se añaden enteros. Un representante puede cubrir divisiones de
  **varios países**: cambiar de país solo cambia la lista, lo añadido se conserva (chips con
  su país cuando hay más de uno). En la
  ficha solo hay dos filas, *Country* y *State*, con todo lo añadido en orden (p. ej.
  "United States, Tuvalu, Togo" y "Florida, Georgia, Nanumaga, Centrale"); su `+` abre *Territory*.
  Los datos van dentro del archivo (unos 55 KB), sin depender de internet.
- **Divisas e idiomas:** siguen **sin vía de entrada**. Si tienen que crecer,
  necesitan el mismo `+` o una pantalla de administración. `languages` y `currencies`
  con toda probabilidad ya existen en el sistema real —la ficha de cliente tiene
  ambos campos—, así que ahí puede estar la respuesta: reutilizar lo que ya los
  administra.

**Software interno › Leads**

- Formulario *New Lead* con el layout del software real y **dos campos** de representante,
  juntos en su fila, ambos de **selección múltiple** y con su **✕** para quitar la selección:
  - **Sales Rep Company** parte del **estado** del lead: si el estado tiene una sola empresa
    representante viene puesta; si tiene varias, no se elige ninguna y el desplegable las
    muestra. *Show all sales reps* amplía a otros territorios, con buscador.
  - **Sales Rep** lista las personas de las empresas elegidas, agrupadas por empresa, con la
    misma lógica: una empresa con un solo contacto lo trae puesto; con varios, se elige.
    Al quitar una empresa se van sus personas.
  *Save* guarda el lead con empresas y personas (la tabla de Leads ya no lleva columna
  *Sales Rep*).
- **Varios representantes en la cotización:** el bloque *Sales Rep* de *General
  Information* usa el mismo desplegable y muestra un *Contact* por empresa. En *Costs*,
  cada empresa aporta el % de su tramo, se suman y el total se reparte a partes iguales:
  3,5 % + 4 % + 3,8 % = 11,3 % → 3,77 % por representante, una comisión por empresa.
- Los `+` junto a *Client* y *Client contact* crean el que falte sin salir del lead.

**Portal de clientes** — `client-portal.html`

Reproduce la página 2 del PDF (*«Software clientes»*) y añade lo que piden sus dos
notas manuscritas.

- **Main Board** con las tres columnas: Bidding Calculator (azul), Active Quotes
  (naranja, con chips *All / PreApproved*) e History (ámbar). Cada cabecera lleva el
  conteo, el total **en la divisa del cliente** y su propio buscador.
- Tarjetas con el nombre del proyecto en negrita, el de la cotización debajo y la
  fecha, tal como en la captura. No se despliegan.
- **Create Quote** replicado campo por campo, con la cascada
  `Country → State → City` que muestra la captura.
- **Documents**: los documentos adjuntos a las cotizaciones del alcance.
- Conmutador `Sign in as: Client / Sales Rep`, y un selector para entrar como
  cualquier cliente o representante. **Abre como representante.**
- Como **cliente**: sin barra de filtros, sin paso de elección, sin bloque
  *Directed quote* y sin mención al representante.
- Como **representante**: al pulsar *New Request* el sistema pregunta primero qué
  tipo de cotización quiere crear:
  - **Directed Quote** — dirigida a uno de sus clientes asociados. Abre el
    formulario con los campos de cliente y contacto.
  - **Quote as Client** — el mismo formulario que usan los clientes, sin
    destinatario. Queda a nombre del representante, con `clientId: null`. Solo está
    habilitado si el representante tiene marcado **Enabled Quote as Client** en su ficha del
    software interno (casilla igual a *Enabled Directed Quotes* del cliente, también en su
    formulario); si no, la opción aparece atenuada. El cambio llega al portal al instante
    (mismo puente de navegador que las solicitudes). Laura Méndez viene sin habilitar.
- Filtro `All / Directed by me` en la misma línea del título, sin recuadro.
- **Solicitud de alta de cliente o contacto** con el `+` junto a *Quote directed to*
  y a *Client contact*. **Cliente nuevo:** nombre, país, estado y ciudad, más un bloque
  *Contacts* con su `+`: cada pulsación abre una fila de nombre y correo que se escribe ahí
  mismo, sin paso intermedio, y un único *Save* guarda el cliente con todas las filas; al
  menos un contacto es obligatorio. **Solo contacto:** nombre y correo, con *Cancel* / *Add contact*.
  El alta se guarda **dentro del borrador**: *Create Quote* no se cierra, el cliente o
  contacto nuevo queda seleccionado como "(new — pending review)" y el representante
  completa el resto del formulario. Con un contacto nuevo, *Quote directed to* deja de
  ser obligatorio y puede elegirse un cliente existente sin perder ese contacto.
  **Solo al guardar el formulario completo** se envía la solicitud (alta + borrador de la
  cotización), aparece en *Bidding Calculator* una tarjeta normal, sin importe ni marca, y el aviso al
  pie: **no se genera cotización ni coste** mientras esté en revisión. La tarjeta se
  conserva al recargar el portal.

## El flujo de una solicitud, de punta a punta

```
Portal de clientes  →  + alta en Create Quote  →  completar el formulario  →  Save
                    →  tarjeta en Bidding Calculator (sin coste)  +  aviso al pie
Software interno    →  tarjeta en Approval con el proyecto del borrador
                       (sin importe, sin pestaña Costs)
                    →  General Information con el mismo formato de cualquier cotización
                    →  nota en Follow Up firmada por Pedro Velez, con los datos
                       pedidos debajo: cliente nuevo con su ubicación y contactos
```

Las dos maquetas son archivos independientes, así que el puente es `localStorage`
bajo el mismo origen: sirviéndolas desde el mismo host el flujo se conecta, y
abiertas sueltas cada una sigue funcionando por su cuenta. **En la implementación
real esto es una colección compartida, no un puente del navegador.**

**Barra superior oscura** (solo maqueta, no es parte del producto)

- `Notas de spec` — reglas de negocio de cada pantalla, con lo nuevo, lo modificado
  y lo pendiente de confirmar.
- `Reiniciar datos` (*Reset data*): vuelve a los datos de ejemplo y **borra las solicitudes de
  prueba** enviadas desde el portal, en las dos maquetas.

## La regla del borde

```
tramo = monto >= umbral ? "above" : "below"
```

Los tramos son `[0, umbral)` y `[umbral, ∞)`. Una cotización de **exactamente**
USD 1.000.000 aplica el porcentaje **superior**: sin hueco, sin solapamiento, y una
sola condición. En la interfaz los campos se rotulan con el operador
(`< $1,000,000` / `≥ $1,000,000`) para que no se reabra la ambigüedad.

El umbral es un campo por representante, no una constante global.

**Base de cálculo — confirmada:** la comisión se calcula sobre el **total de la
cotización**. Ese total es también el que decide el tramo, así que umbral y base son
la misma cifra: no hay caso en que una cotización caiga en el tramo superior pero
comisione sobre un número distinto. Se guarda en `commissionSnapshot.basisUsd`.

## Modelo de datos propuesto

```
cities/{cityId}                       name, code, active
currencies/{currencyId}               name, code, symbol, active
languages/{languageId}                name, code, flag, active

salesReps/{repId}                     name, email, phone, active, standard, authUid,
                                      cityIds [ ], currencyId, languageId, state,
                                      contacts [ { id, name, role?, email } ],
                                      margin, accMargin, commission,
                                      tiers { thresholdUsd, belowPct, abovePct }

salesRepClients/{repId}_{clientId}    repId, clientId, assignedAt, assignedBy

quotes/{quoteId}                      + createdByType, salesRepId, salesRepCityId,
                                        targetClientId, targetContactId,
                                        commissionSnapshot { pct, tier, basisUsd,
                                                             currencyId, appliedAt }
```

**Dos comisiones en la misma ficha, y no son lo mismo.** El representante puede
cotizar de dos maneras, así que lleva las dos:

| Campo | Cuándo aplica | Quién la cobra |
| --- | --- | --- |
| `commission` (un solo %) | Cuando el representante cotiza **como cliente**, sin destinatario | Es su condición comercial, igual que la de cualquier cliente |
| `tiers` (dos tramos) | Cuando el representante **dirige** una cotización a un cliente asociado | Es su comisión de venta |

Las mismas condiciones que tiene un cliente —`margin`, `accMargin`, `commission`—
viven también en el representante por ese motivo. En la interfaz cada una lleva su
rótulo: *applies when quoting as client* y *earned on quotes directed to a client*.

**Qué se congela en la cotización y qué no.** Ciudad y divisa sí: son datos del negocio
cerrado y van al `commissionSnapshot`. El idioma no: es una preferencia del representante
que puede cambiar sin consecuencias contables.

**Sobre `languages`:** la ficha de cliente ya tiene un campo *Language*. Ese catálogo
probablemente ya existe en el sistema — hay que **reutilizarlo**, no crear uno paralelo.
Es lo primero que habría que verificar contra el repositorio real.

La asociación va en una **colección de unión**, no en un array dentro del representante:
se consulta en los dos sentidos, no topa con el límite de tamaño del documento, y el ID
compuesto permite que una regla de Firestore la verifique con un `exists()` en O(1).

La ciudad y el porcentaje se **copian** a cada cotización al crearla. Si mañana se editan
los porcentajes del representante, el histórico no se mueve.

## Seguridad

El desplegable filtrado es comodidad, no seguridad. Las cinco restricciones del brief se
hacen cumplir en el backend:

| Restricción | Dónde se hace cumplir |
| --- | --- |
| Solo ve sus clientes | `allow read … assigned(clientId)` |
| Solo cotiza para sus clientes | `allow create … assigned(targetClientId)` |
| No modifica asociaciones | `salesRepClients: allow write if isInternalUser()` |
| No modifica sus porcentajes | `salesReps: allow write if isInternalUser()` |
| Los % no vienen del payload | Los escribe la Cloud Function leyendo el documento del rep |

El rol viaja en **custom claims** de Firebase Auth, firmados dentro del ID token: las
reglas lo leen sin gastar un `get()` y no hay forma de alterarlo desde el navegador.

## Decisiones confirmadas

- **Base de cálculo de la comisión: el total de la cotización.** Confirmado por Natalia
  el 8 de septiembre de 2026. Era la única pendiente que cambiaba el dinero a pagar.
- **«Directed Quotes» se extiende, no se duplica.** El representante reutiliza el
  mecanismo ya especificado; el discriminador es `createdByType`.
- **Un representante puede tener uno o varios clientes asociados.** La colección
  de unión lo soporta sin límite y la maqueta lo ejercita: en los datos de ejemplo
  hay representantes con 3 clientes y con 1.

## Consecuencia abierta: quién inicia sesión en el portal

`salesReps.authUid` era un login por representante, cuando el representante era
una persona. Ahora que es una **empresa**, la pregunta es quién entra al portal.

Lo más probable es que cada contacto necesite su propio acceso, lo que mueve
`authUid` de la empresa al contacto:

```
salesReps/{repId}/contacts[]          + authUid   ← el login vive aquí
```

Eso decide además si la cotización debe registrar **qué persona** la creó
(`salesRepContactId`) y no solo qué firma. La comisión seguiría siendo de la
empresa, pero el seguimiento comercial sería por persona.

El portal (`client-portal.html`) **todavía trata al representante como persona**;
hay que alinearlo una vez se decida esto.

## Conflicto abierto: qué gobierna la elección del representante

En el formulario de *New Lead*, el desplegable **Sales Rep** se condiciona a la
**ciudad del cliente** seleccionado. Esa regla choca de frente con la colección
`salesRepClients`, donde un representante se asigna a clientes **con independencia
de la ciudad**.

Se ve en los propios datos de ejemplo: Michael Grant es de Tampa y tiene asignado
*1st Choice Glass*, que está en Nashville. Al elegir ese cliente, el desplegable
responde *«No sales reps in Nashville»* — es decir, **no puede elegirse al
representante de su propio cliente**.

Una de las dos reglas tiene que ceder:

- **Filtrar por asignación** (`salesRepClients`) en vez de por ciudad. Es coherente
  con el resto del módulo y con el portal, donde el aislamiento se basa en la
  asignación.
- **O prohibir asignaciones fuera de la ciudad** del representante, lo que convierte
  la ciudad en una frontera comercial dura y obliga a revisar las asignaciones
  actuales.

## Decisiones pendientes de confirmación

1. ¿Un cliente puede tener más de un representante? (la maqueta asume que sí)
2. ~~¿Un representante puede cubrir varias ciudades?~~ **Cubre estados, varios** (29 sep 2026).
3. ¿Qué significa «sus cotizaciones» en el filtro del tablero?
4. ¿Puede un rep cotizar sin cliente destinatario? (asume que no)
5. ¿Qué ve el cliente de una cotización que dirigió su representante?
6. Al desasociar un cliente, ¿el rep pierde acceso a lo ya creado?
7. ¿Un lead con representante genera comisión, o solo la cotización que salga de él?
8. Si la divisa del representante difiere de la del cliente, ¿a qué tasa se convierte
   la comisión y en qué momento se fija?
9. ¿Un idioma o una divisa deben poder desactivarse estando en uso?
10. ¿Qué pasa si un representante queda inactivo con cotizaciones vivas?

Ninguna de las que quedan bloquea el arranque ni cambia el dinero que se paga.
El detalle de cada una está en `sales-rep-analysis.html`.

## Dos hallazgos que conviene revisar antes de programar

**Ya existe «Directed Quotes».** El checkbox *Enabled Directed Quotes for Client* de la
ficha de cliente y el PDF *Cotizaciones Dirigidas* describen un mecanismo ya especificado.
La decisión tomada es **extenderlo**, no duplicarlo: una cotización dirigida sigue siendo
una cotización dirigida y lo que cambia es quién la origina. La consecuencia es que esa
casilla pasa a controlar también si un representante puede cotizarle a ese cliente — hay
que avisar a quien la administra hoy.

**El cliente ya tiene un campo `Commission` (1 %).** Es otro concepto, con otro beneficiario
y otra base. No unificar los nombres.

## Requisitos que estaban en el PDF pero no en el brief escrito

Dos notas manuscritas sobre las capturas, ambas implementadas en la maqueta:

- Sobre el Main Board: *«un filtro o una sección donde el representante de ventas pueda ver
  sus cotizaciones y sus cotizaciones dirigidas»*.
- Sobre el formulario de leads: *«añadir sección de Sales Rep»*.

## Aviso

El análisis se hizo **sin acceso al código fuente**. La arquitectura actual está
reconstruida a partir de las capturas del PDF y de la URL visible en una de ellas
(`projectagenda.com/clients/PzA1Vo7OX5fXxXklQfQc/general-information`, cuyo identificador
de 20 caracteres apunta a Firestore). Todo lo descrito como «actual» debe verificarse
contra el repositorio real antes de implementar.
