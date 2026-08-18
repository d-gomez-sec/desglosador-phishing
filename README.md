**▶ Pruébala aquí:** https://d-gomez-sec.github.io/desglosador-phishing/
# Desglosador de Phishing

Pega el texto de un mensaje sospechoso y te señala, una por una, las técnicas de manipulación que usa y por qué funcionan.

**Qué hace:** en lugar de dar un veredicto opaco de "esto es phishing", muestra dónde está el engaño dentro del propio texto, para que quien lo use aprenda a reconocerlo sin la herramienta.
**Nota honesta:** el eslabón que decide si un fraude prospera es la persona que lee el mensaje. Entrenar esa lectura es más valioso que cualquier herramienta.

---

## Cómo funciona

Pegas el mensaje completo —correo o mensaje directo, tal cual, sin separar nada— y obtienes tres cosas:

**El mensaje marcado.** El texto original con las frases problemáticas resaltadas por color según la familia de técnica, con su leyenda.

**Las técnicas explicadas.** Una tarjeta por familia detectada, ordenadas por gravedad, con la palanca psicológica que utiliza y los fragmentos concretos que la disparan:

| Familia | Qué busca |
|---|---|
| Petición de datos | Credenciales, datos bancarios, "inicia sesión aquí" — el objetivo final del robo |
| Miedo y amenaza | Cuenta suspendida, actividad sospechosa, multas |
| Urgencia | Plazos, "última oportunidad", presión temporal |
| Cebo emocional | Premios, reembolsos, peticiones de ayuda o apoyo |
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

## Idioma

Base en español con las señales más frecuentes en inglés, porque muchos fraudes llegan traducidos automáticamente o directamente sin traducir.

---

## Uso

Abre `index.html` en cualquier navegador. Un solo archivo, sin instalación, sin conexión y sin dependencias. El texto analizado no sale del dispositivo.

Incluye un botón que carga el caso real de ejemplo, para ver cómo razona antes de usarlo con mensajes propios.

---

## Limitaciones conocidas

**Analiza patrones conocidos, no comprende el mensaje.** Un fraude bien redactado, sin prisas ni errores, puede pasar sin disparar señales. La ausencia de avisos no es un certificado de seguridad, y la herramienta lo dice explícitamente en pantalla en lugar de dejarlo en la documentación.

El catálogo de marcas y de patrones es finito: una suplantación de una marca no incluida no activará el careo. Y al trabajar sobre coincidencias de texto, una redacción inusual puede escapársele.

Es un apoyo para entrenar el criterio, no un filtro automático.

---

## Qué aprendí construyéndolo

- **Una herramienta que da veredictos crea dependencia; una que explica crea criterio.** El diseño cambió al asumir que el objetivo era volverse innecesaria: quien la use veinte veces debería reconocer los patrones sin ella.
- **Las herramientas técnicas no cubren el engaño en texto plano.** Un caso real lo dejó claro: el análisis de dominio daba riesgo cero y era correcto, porque el dominio no imitaba a nadie. El fraude estaba en que el mensaje decía representar a dos empresas que no tenían nada que ver con el enlace. Ninguna comprobación técnica del dominio o de las cabeceras alcanza esa capa.
- **Declarar los límites es parte de la herramienta.** Un analizador que no avisa de lo que no puede ver induce a confiar de más, y en seguridad esa confianza es el fallo, no la comodidad.

---

## Licencia

MIT — ver [LICENSE](LICENSE).
