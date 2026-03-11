# I - Introducción

En la era de la inteligencia artificial, donde la entrada en al desarrollo de aplicaciones es tan simple, donde el código es cada ves más barato, los que prevalecerán serán aquellos que vean o sientan al desarrollo de software como un arte. 

Serán aquellos que tengan una filosofía de desarrollo, que piensen y diseñen como artistas. Llegará un punto donde las IAs serán tan poderosas que las principales diferencias se verán en lo minúsculo. 

Todo pasará a ser similar, carente de profundidad y principios, el software creado será castillos de naipes o torres de jenga, donde si se mueve el naipe o si se saca el bloque de jenga equivocado, todo se cae.

Pero aquellos que tengan una filosofía de desarrollo, los que se crean artistas, los que le preste atención a los detalles serán los diferentes, serán los Davincis o los Musks de la época.Su software pasaran a ser cuadros que admiraran e intentaran interpretar. Serán productos robustos, con una impronta propia del ser humano reflejarán sus gustos y su visión.

A continuación paso a describir mi filosofía de desarrollo. Probablemente cambie a lo largo del tiempo, a medida que vaya adquiriendo nuevos conocimientos y experiencias. 

El objetivo es que sea un manifiesto donde dejo plasmado cómo creo todo el software, desde el momento que escribo este manifiesto hasta el día que me muera.

# II - La verdad fundamental

> El software existe para resolver problemas humanos, y debe poder evolucionar mientras lo hace.

Este es el axioma irreducible en el que está sostenido todo lo demás. Si el software no resuelve un problema humano, no debería existir. Si no puede evolucionar morirá

## El modelo

Del axioma anterior se deriva:
> El software es un modelo ejecutable de la realidad. Su estructura debe reflejar la estructura del problema, no la solución técnica.

¿Por qué? Porque los problemas humanos existen en dominios humanos. Si el código refleja el dominio, cuando el dominio evoluciona, sabes exactamente donde cambiar. Si refleja la tecnología en cada cambio de negocio requiere arqueología técnica nueva.

# III - La ley de Localidad

> Cada decisión, cada dato, cada comportamiento debe vivir en exactamente un lugar - el lugar donde tiene sentido conceptualmente - y ese lugar debe poder cambiar sin propagar ondas de choque al resto del sistema.

La evolución requiere cambio y el cambio solo es sostenible si es local. Si mover una pieza derrumba el edificio, el edificio no puede evolucionar.

## Los 3 pilares

De la ley de Localidad surgen 3 pilares

### Pilar I: Responsabilidad Única Localizada

Un módulo existe si y solo sí:
1. Tiene responsabilidad clara e indivisible
2. Esa responsabilidad no puede vivir en otro lugar sin aumentar el acoplamiento
3. Sin él, el sistema pierda una capacidad esencial

### Pilar II: Cohesión por Razón de Cambio

Agrupar código NO por similitud técnica, sino por volatilidad compartida

- Lo que cambia junto, vive junto
- Lo que cambia por razones distintas, se separa.

### Pilar III: Acoplamiento Intencional

El acoplamiento no es malo. El acoplamiento ACCIDENTAL es malo

Cada dependencia debe ser:

- Explícita (visible en el contrato)
- Direccional (flujo claro)
- Justificada (resuelve un problema real)
- Mínima (solo lo necesario para colaborar)
Se deben asignar las responsabilidades de manera que su localización no incremente el acoplamiento hasta un nivel que produzca efectos negativos

# III - Modularidad

> La modularidad es la propiedad de un sistema que se ha descompuesto en un conjunto de módulos cohesivos y débilmente acoplados

La modularidad se alcanza:

- Diseñando cada método con un único y claro objetivo
- Agrupando un conjunto de aspectos relacionados en una clase
- Organizando capacidades del sistema en módulos autónomos
- Manteniendo dependencias explícitas y mínimas entre módulos
