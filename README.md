# Desglosador de Phishing
**▶ Pruébala aquí:** https://d-gomez-sec.github.io/desglosador-phishing/

Pega el texto de un mensaje sospechoso y te señala, una por una, las técnicas de manipulación que usa y por qué funcionan. Con modo específico para SMS.

**Qué hace:** en lugar de dar un veredicto opaco de "esto es phishing", muestra dónde está el engaño dentro del propio texto, para que quien lo use aprenda a reconocerlo sin la herramienta.
**Nota honesta:** el eslabón que decide si un fraude prospera es la persona que lee el mensaje. Entrenar esa lectura es más valioso que cualquier herramienta.

![Ejemplo de mensaje de phishing](docs/mensaje-phishing.png)

---

## Dos modos

Un selector arriba elige el tipo de mensaje. El análisis de texto es común; el modo SMS añade tres capas que solo tienen sentido en ese canal.

| | Correo o mensaje directo | SMS |
|---|---|---|
| Análisis del texto | ✓ | ✓ |
| Análisis de enlaces | ✓ | ✓ |
| Careo marca ↔ enlace | ✓ | ✓ |
| Análisis del remitente | — | ✓ |
| Contraste con el canal oficial | — | ✓ |
| Señales propias del canal | — | ✓ |
| Qué hacer ahora | — | ✓ |

---

## Cómo funciona

Pegas el mensaje completo, tal cual, sin separar nada, y obtienes:

**El mensaje marcado.** El texto original con las frases problemáticas resaltadas por color según la familia de técnica, con su leyenda.

**Las técnicas explicadas.** Una tarjeta por familia detectada, ordenadas por gravedad, con la palanca psicológica que utiliza y los fragmentos concretos que la disparan:

| Familia | Qué busca |
|---|---|
| Petición de datos | Credenciales, datos bancarios, "inicia sesión aquí" — el objetivo final del robo |
| Petición de un código | "Pásame el código que te llegó" — el robo de cuenta de WhatsApp |
| Miedo y amenaza | Cuenta suspendida, actividad sospechosa, multas |
| Urgencia | Plazos, "última oportunidad", presión temporal |
| Cebo emocional | Premios, reembolsos, peticiones de ayuda o apoyo |
| Suplantación de un familiar | "Hola mamá, se me ha roto el móvil, este es mi nuevo número" |
| Autoridad prestada | Nombres de empresas u organismos de confianza |
| Saludo genérico | "Estimado cliente" — indica envío masivo |

**El resumen.** Teje las señales encontradas en una lectura de conjunto, sin una puntuación numérica que dé falsa sensación de precisión.

### El careo marca ↔ enlace

La señal principal. Si el texto invoca a una marca conocida pero ningún enlace del mensaje lleva a un dominio oficial de esa marca, lo marca como suplantación. Es el desajuste entre **lo que dice ser** y **a dónde te lleva**, y no lo detecta ni el análisis de cabeceras ni el de dominios: solo aparece al cruzar el texto con los enlaces.

El caso de ejemplo incluido es real: un mensaje que dice representar a Spotify y Google en un concurso de pódcast, con un enlace a un dominio sin relación alguna con ninguna de las dos.

### Análisis de enlaces

Extrae los enlaces del texto automáticamente y señala lo que es visible sin visitarlos: acortadores, `http://` sin cifrar, IP cruda como destino, una marca usada como subdominio de otro dominio (`paypal.algo-raro.com`), y dominios con varios guiones.

**No sigue las redirecciones**, porque el navegador no lo permite. En su lugar remite a un expansor de enlaces y a un detector de homógrafos para completar el análisis.

---

## El modo SMS

### Análisis del remitente

Clasifica el número o el nombre **por su estructura**, no por su identidad: no consulta ninguna base de datos ni envía el número a ningún servicio.

- **Móvil personal español** — ningún organismo comunica así; una SIM de prepago cuesta unos euros y se desecha en días
- **Número extranjero** — un organismo español no escribe desde otro país
- **Número corto de servicio** — más difícil de conseguir, pero también se alquila
- **Nombre en vez de número** — el caso más importante: la cabecera alfanumérica **se falsifica**, y si coincide con la de mensajes legítimos, el móvil los agrupa **en el mismo hilo** que los auténticos. Que ponga "AEAT" o el nombre de tu banco no prueba nada

### Contraste con el canal oficial

Eliges quién dice ser el mensaje y la herramienta muestra qué hace y qué **nunca** hace ese emisor, más cómo verificarlo por un canal que inicies tú. Trece perfiles, elegidos por frecuencia real en España:

Agencia Tributaria · Correos y paquetería · bancos · Seguridad Social y SEPE · DGT · compañías de luz y gas · Apple, Google, Microsoft y Amazon · **un familiar** ("Hola mamá, soy yo") · operadoras de telefonía · **WhatsApp y el código de verificación** · **Bizum** · **compraventa de segunda mano** · y una entrada genérica con las reglas universales para lo que no esté en la lista.

Ese contraste es lo que resuelve el caso en segundos: la AEAT nunca envía enlaces para cobrar una devolución, nadie necesita que aceptes una solicitud para *enviarte* dinero, y el código de verificación no se comparte con nadie bajo ninguna excusa.

### Señales propias del canal

- **El acortador en un SMS** — casi obligatorio por el límite de caracteres, así que por sí solo no condena; pero oculta el destino justo donde la pantalla es pequeña y no puedes previsualizar
- **El slug fabricado** — una ruta tipo `bit.ly/AEAT-F451371` imita una referencia de expediente. En un acortador cualquiera escribe lo que quiera ahí: no acredita nada
- **El importe con céntimos** — parece específico y por eso resulta creíble, pero es el mismo para toda la campaña
- **La brevedad** — es una restricción del canal, no una señal de legitimidad

### Qué hacer ahora

El modo SMS cierra con los pasos concretos: no responder (responder confirma que el número está activo), verificar por un canal propio, reenviar al **7726** —el número gratuito de las operadoras españolas para reportar smishing—, bloquear, reportar al INCIBE en el **017**, y qué hacer en las primeras horas si ya se pulsó y se introdujeron datos.

---

## Idioma

Base en español con las señales más frecuentes en inglés, porque muchos fraudes llegan traducidos automáticamente o directamente sin traducir.

---

## Uso

Abre `index.html` en cualquier navegador. Un solo archivo, sin instalación, sin conexión y sin dependencias. El texto analizado no sale del dispositivo.

Cada modo incluye un botón que carga un caso real de ejemplo, para ver cómo razona antes de usarlo con mensajes propios. El del modo SMS es una campaña real que suplantaba a la Agencia Tributaria.

---

## Limitaciones conocidas

**Analiza patrones conocidos, no comprende el mensaje.** Un fraude bien redactado, sin prisas ni errores, puede pasar sin disparar señales. La ausencia de avisos no es un certificado de seguridad, y la herramienta lo dice explícitamente en pantalla en lugar de dejarlo en la documentación.

El catálogo de marcas y de patrones es finito: una suplantación de una marca no incluida no activará el careo. Y al trabajar sobre coincidencias de texto, una redacción inusual puede escapársele.

**El análisis del remitente no identifica a nadie.** Dice qué tipo de número es y qué implica, no de quién es. Averiguar el titular exige servicios de pago o entregar el número a terceros, que es justo lo que la herramienta evita.

Los canales oficiales son de organismos y empresas de **España**.

Es un apoyo para entrenar el criterio, no un filtro automático.

---

## Qué aprendí construyéndolo

- **Una herramienta que da veredictos crea dependencia; una que explica crea criterio.** El diseño cambió al asumir que el objetivo era volverse innecesaria: quien la use veinte veces debería reconocer los patrones sin ella.
- **Las herramientas técnicas no cubren el engaño en texto plano.** Un caso real lo dejó claro: el análisis de dominio daba riesgo cero y era correcto, porque el dominio no imitaba a nadie. El fraude estaba en que el mensaje decía representar a dos empresas que no tenían nada que ver con el enlace. Ninguna comprobación técnica del dominio o de las cabeceras alcanza esa capa.
- **Declarar los límites es parte de la herramienta.** Un analizador que no avisa de lo que no puede ver induce a confiar de más, y en seguridad esa confianza es el fallo, no la comodidad.
- **El canal cambia el análisis, no solo el formato.** El modo SMS nació de un caso real de smishing donde el resto de comprobaciones no llegaban: no había cabeceras que leer y el remitente era un móvil cualquiera. Un motor de texto no basta cuando el vector tiene sus propias reglas.
- **Saber qué hace el emisor legítimo vale más que analizar al falso.** La comprobación más rápida no es examinar el mensaje sospechoso, sino saber que la Agencia Tributaria nunca manda enlaces de cobro. Por eso la tabla de canales oficiales acabó siendo la parte más útil de todo el modo SMS.
- **Los fraudes sin enlaces obligaron a replantear el motor.** La suplantación de un familiar y el robo del código de WhatsApp no traen enlaces ni marcas, así que pasaban casi en blanco por un analizador construido alrededor de las URL. Detectarlos exigió patrones de conversación, no de infraestructura.

---

## Licencia

MIT — ver [LICENSE](LICENSE).
