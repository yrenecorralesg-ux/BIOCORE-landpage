<script lang="ts">
  type FormStatus = 'idle' | 'error' | 'success'

  const photos = {
    uneg: '/images/image.png',
    kit: '/images/Imagen_de_ChatGPT_29_sept_2026,_08_32_29.png',
    hero: 'https://images.pexels.com/photos/32738814/pexels-photo-32738814.jpeg?auto=compress&cs=tinysrgb&h=1200&w=1600',
    soil: 'https://images.pexels.com/photos/31110992/pexels-photo-31110992.jpeg?auto=compress&cs=tinysrgb&h=900&w=1200',
    community: 'https://images.pexels.com/photos/36869075/pexels-photo-36869075.jpeg?auto=compress&cs=tinysrgb&h=900&w=1200',
    research: 'https://images.pexels.com/photos/5622487/pexels-photo-5622487.jpeg?auto=compress&cs=tinysrgb&h=900&w=1200'
  }

  const navItems = [
    { label: 'Solución', href: '#solucion' },
    { label: 'Cómo funciona', href: '#como-funciona' },
    { label: 'El kit', href: '#kit' },
    { label: 'Aplicación', href: '#aplicacion' },
    { label: 'Impacto', href: '#impacto' },
    { label: 'Preguntas frecuentes', href: '#faq' }
  ]

  const processSteps = [
    { number: '01', title: 'Biodigestor', text: 'El proceso biológico ocurre en un sistema cuya operación necesita observación contextual.' },
    { number: '02', title: 'Sensores', text: 'Recogen señales del lugar donde se instalan para construir una lectura ordenada.' },
    { number: '03', title: 'ESP32', text: 'El módulo recibe las señales y las prepara para una consulta local o sincronizada.' },
    { number: '04', title: 'Registro', text: 'Los datos se conservan con fecha para comparar cambios y documentar el proceso.' },
    { number: '05', title: 'Aplicación', text: 'BIOCORE presenta el estado, el historial y las alertas de forma comprensible.' }
  ]

  const audiences = [
    { title: 'Comunidades rurales', text: 'Registros compartidos y una lectura más clara para acompañar el uso cotidiano del biodigestor.', image: photos.community, alt: 'Personas trabajando en un campo agrícola verde' },
    { title: 'Productores', text: 'Seguimiento del proceso y señales de revisión para tomar decisiones con más contexto.', image: photos.soil, alt: 'Mano sosteniendo material orgánico para uso agrícola' },
    { title: 'Instituciones y universidades', text: 'Datos ordenados para prácticas, ensayos y proyectos de investigación reproducibles.', image: photos.research, alt: 'Investigadora usando una tableta entre plantas' }
  ]

  const faqs = [
    { question: '¿Qué incluye BIOCORE?', answer: 'La propuesta reúne un módulo de monitoreo con ESP32, sensores definidos según la instalación y acceso a una aplicación para consultar registros. La configuración final pertenece a la etapa de diseño y validación.' },
    { question: '¿Necesito internet para usarlo?', answer: 'La próxima versión contempla una consulta local cerca del equipo y una modalidad conectada para sincronizar datos. El Wi-Fi local del ESP32 por sí solo no da acceso a la aplicación publicada en la nube.' },
    { question: '¿Sirve para cualquier biodigestor?', answer: 'No existe una configuración universal. El tipo, el tamaño, la ubicación y las condiciones del biodigestor definen qué sensores y montaje son adecuados.' },
    { question: '¿Mide cuántos litros de biogás se producen?', answer: 'No. El MVP no mide volumen de gas ni porcentaje de metano. El MQ-2 entrega una señal relativa ante gases combustibles y el SHT31 observa temperatura y humedad donde se instala.' },
    { question: '¿Cómo solicito un piloto?', answer: 'Completa el formulario con la información de tu organización, ubicación y biodigestor. Revisaremos el caso para conversar sobre una posible evaluación técnica.' }
  ]

  let currentPath = typeof window !== 'undefined' ? window.location.pathname : '/'
  let menuOpen = false
  let openFaq = -1
  let formStatus: FormStatus = 'idle'
  let formMessage = ''
  let formData = {
    name: '',
    email: '',
    phone: '',
    organization: '',
    location: '',
    digester: '',
    need: '',
    consent: false
  }

  function scrollToRequest() {
    document.querySelector('#solicitar')?.scrollIntoView({ behavior: 'smooth' })
    menuOpen = false
  }

  function handleSubmit(event: SubmitEvent) {
    event.preventDefault()
    if (!formData.consent) {
      formStatus = 'error'
      formMessage = 'Necesitamos tu consentimiento para poder contactarte.'
      return
    }
    formStatus = 'success'
    formMessage = 'Recibimos tu solicitud. El equipo de BIOCORE se pondrá en contacto contigo para conocer mejor el caso.'
  }
</script>

<svelte:head>
  <title>BIOCORE | Monitoreo para biodigestores rurales</title>
  <meta name="description" content="BIOCORE reúne sensores y una aplicación para consultar el estado del proceso en biodigestores rurales y conservar registros trazables." />
  <meta property="og:title" content="BIOCORE | Monitoreo para biodigestores rurales" />
  <meta property="og:description" content="Comprende lo que ocurre dentro de tu biodigestor." />
  <meta property="og:image" content={photos.hero} />
</svelte:head>

{#if currentPath === '/login'}
  <main class="login-page">
    <a class="brand brand-dark" href="/">BIOCORE<span>.</span></a>
    <section class="login-card" aria-labelledby="login-title">
      <p class="eyebrow">Acceso privado</p>
      <h1 id="login-title">Entra a tu espacio BIOCORE</h1>
      <p>El acceso a la aplicación se habilita para equipos y pilotos activos.</p>
      <label>Correo electrónico<input type="email" placeholder="tu@organizacion.org" /></label>
      <label>Contraseña<input type="password" placeholder="••••••••" /></label>
      <button class="button button-primary full" type="button">Iniciar sesión</button>
      <a class="back-link" href="/">Volver a la página pública</a>
    </section>
  </main>
{:else}
  <header class="site-header">
    <div class="nav-wrap">
      <a class="brand" href="/" aria-label="BIOCORE inicio">BIOCORE<span>.</span></a>
      <button class="menu-toggle" type="button" aria-label="Abrir menú" aria-expanded={menuOpen} on:click={() => (menuOpen = !menuOpen)}>
        <span></span><span></span>
      </button>
      <nav class:open={menuOpen} aria-label="Navegación principal">
        {#each navItems as item}
          <a href={item.href} on:click={() => (menuOpen = false)}>{item.label}</a>
        {/each}
        <a class="login-link" href="/login">Iniciar sesión</a>
        <button class="button button-small" type="button" on:click={scrollToRequest}>Solicitar BIOCORE</button>
      </nav>
    </div>
  </header>

  <main>
    <section class="hero" aria-labelledby="hero-title">
      <div class="hero-copy">
        <div class="eyebrow light"><span class="eyebrow-dot"></span> Del prototipo experimental a una solución de monitoreo rural</div>
        <h1 id="hero-title">Comprende lo que ocurre <em>dentro de tu biodigestor</em></h1>
        <p class="hero-text">BIOCORE reúne sensores y una aplicación para consultar el estado del proceso, conservar registros y preparar mejores decisiones en campo.</p>
        <div class="hero-actions">
          <button class="button button-accent" type="button" on:click={scrollToRequest}>Solicitar BIOCORE <span>↗</span></button>
          <a class="text-button light" href="#como-funciona">Conocer cómo funciona <span>↓</span></a>
        </div>
        <div class="hero-note"><span class="line"></span> Una propuesta para observar antes de intervenir.</div>
      </div>
      <div class="hero-visual">
        <img src={photos.hero} alt="Biodigestor rodeado de cultivos y paisaje rural" />
        <div class="hero-stamp"><strong>01</strong><span>Monitoreo<br />rural</span></div>
        <div class="hero-caption">Biodigestor + sensores + registro</div>
      </div>
    </section>

    <section class="uneg-section section-shell" aria-labelledby="uneg-title">
      <div class="uneg-logo-wrap"><img src={photos.uneg} alt="Logotipo oficial de la Universidad Nacional Experimental de Guayana" /><span class="uneg-campus">UNIVERSIDAD NACIONAL EXPERIMENTAL DE GUAYANA<strong>SEDE PUERTO ORDAZ</strong></span></div>
      <div class="uneg-copy"><div class="section-kicker">Origen académico</div><h2 id="uneg-title">Innovación desde la UNEG para un campo más sostenible</h2><p>BIOCORE nació como un trabajo de investigación de Ingeniería Informática en la Universidad Nacional Experimental de Guayana. Integra tecnología, aprovechamiento responsable de residuos orgánicos y una propuesta de monitoreo pensada para contextos rurales.</p><div class="authors"><span>Autores</span><strong>Yrene Corrales <i>·</i> Bryan Salazar</strong></div><div class="uneg-phrase">Ingeniería informática aplicada a la autosostenibilidad y al cuidado del ambiente</div></div><div class="uneg-photo"><img src={photos.community} alt="Paisaje agrícola rural con vegetación abundante" /></div>
    </section>

    <section class="intro section-shell" id="solucion" aria-labelledby="problem-title">
      <div class="section-kicker">El punto de partida</div>
      <div class="split-heading">
        <h2 id="problem-title">Cuando el proceso no deja registro, cada decisión empieza de nuevo.</h2>
        <p>Muchos biodigestores artesanales se operan mediante observación ocasional. Sin una bitácora continua, es difícil relacionar lo que se ve hoy con lo que ocurrió antes.</p>
      </div>
      <div class="problem-grid">
        <article class="problem-card problem-image">
          <img src={photos.soil} alt="Material orgánico sostenido en una mano en un entorno agrícola" />
          <div class="image-overlay"><span>La experiencia importa.</span><strong>El registro también.</strong></div>
        </article>
        <article class="problem-card"><span class="card-number">01</span><h3>Condiciones difíciles de interpretar</h3><p>Temperatura, humedad y señales ante gases combustibles pueden cambiar según el entorno y la etapa del proceso.</p></article>
        <article class="problem-card warm"><span class="card-number">02</span><h3>Sin una línea de tiempo</h3><p>La disponibilidad de gas y las situaciones que requieren revisión quedan sujetas a impresiones aisladas.</p></article>
        <article class="problem-card dark-card"><span class="card-number">03</span><h3>Observar para aprender</h3><p>BIOCORE propone convertir observaciones de campo en una historia consultable, sin prometer más de lo que los sensores pueden validar.</p></article>
      </div>
    </section>

    <section class="process-section" id="como-funciona" aria-labelledby="process-title">
      <div class="section-shell">
        <div class="section-kicker light">El sistema</div>
        <div class="split-heading light-heading"><h2 id="process-title">Una cadena simple para hacer visible el proceso.</h2><p>Desde el biodigestor hasta la aplicación, cada etapa tiene un papel concreto. La información se presenta como apoyo para observar, no como un diagnóstico automático.</p></div>
        <div class="process-flow">
          {#each processSteps as step, index}
            <article class="process-step">
              <div class="step-top"><span>{step.number}</span>{#if index < processSteps.length - 1}<i></i>{/if}</div>
              <div class="step-icon" aria-hidden="true">{index === 0 ? '◒' : index === 1 ? '⌁' : index === 2 ? '▣' : index === 3 ? '▤' : '⌂'}</div>
              <h3>{step.title}</h3><p>{step.text}</p>
            </article>
          {/each}
        </div>
        <div class="sensor-note"><strong>Lecturas con contexto</strong><span>El SHT31 observa temperatura y humedad en el lugar donde se instala. El MQ-2 entrega una señal relativa ante gases combustibles; no es un medidor de volumen ni de porcentaje de metano.</span></div>
      </div>
    </section>

    <section class="kit-section section-shell" id="kit" aria-labelledby="kit-title">
      <div class="section-kicker">La próxima pieza</div>
      <div class="split-heading"><h2 id="kit-title">BIOCORE: tecnología preparada para llegar al campo</h2><p>Una propuesta de integración con módulo ESP32, sensores definidos según la instalación, alimentación, carcasa protectora y acceso a la plataforma. La imagen comunica una dirección de diseño, no un equipo fabricado, certificado o instalado.</p></div>
      <div class="kit-layout">
        <div class="kit-stage kit-image-stage">
        <div class="concept-label">Diseño conceptual de la próxima versión</div>
        <img class="kit-product-image" src={photos.kit} alt="Diseño conceptual de un módulo BIOCORE verde con pantalla y cableado protegido" />
        <p class="render-note">Diseño conceptual del módulo integrado</p>
      </div>
        <div class="kit-details">
          <article><span class="detail-index">01</span><div><h3>Caja protectora</h3><p>Un módulo central pensado para resguardar la electrónica en el entorno de trabajo.</p></div></article>
          <article><span class="detail-index">02</span><div><h3>ESP32 integrado</h3><p>Recibe señales, habilita una consulta cercana y prepara la sincronización.</p></div></article>
          <article><span class="detail-index">03</span><div><h3>Sensores externos</h3><p>Conexiones ordenadas para ajustar la instrumentación a cada biodigestor.</p></div></article>
          <article><span class="detail-index">04</span><div><h3>Alimentación</h3><p>La solución final definirá el esquema de energía según el lugar de instalación.</p></div></article>
        </div>
      </div>
      <div class="mvp-grid"><article class="mvp-card"><span class="status-tag done">Realizado</span><h3>MVP actual: montaje experimental en envases de 15 L</h3><p>Una base de prueba para observar el comportamiento del sistema y ordenar el aprendizaje técnico.</p></article><article class="mvp-card proposed"><span class="status-tag next">Propuesta</span><h3>Próxima fase: piloto rural cercano a 500 L</h3><p>Una instalación de campo para evaluar el montaje, el contexto de uso y la utilidad de los registros.</p></article></div>
    </section>

    <section class="app-section" id="aplicacion" aria-labelledby="app-title">
      <div class="section-shell app-layout"><div class="app-copy"><div class="section-kicker light">La aplicación</div><h2 id="app-title">Del biodigestor a una lectura comprensible</h2><p>BIOCORE organiza las variables disponibles, el historial y los eventos del proceso en una misma experiencia de consulta. Así, cada registro puede acompañarse de su fecha, origen y estado.</p><div class="app-features"><span><b>01</b> Variables disponibles</span><span><b>02</b> Historial del proceso</span><span><b>03</b> Alertas y reportes</span></div><p class="account-note">El acceso requiere una cuenta habilitada.</p></div><div class="dashboard-mockup biocore-dashboard"><div class="platform-window"><div class="platform-header"><div><strong>BIOCORE</strong><span>Panel de monitoreo</span></div><div class="platform-nav"><span class="selected">Resumen</span><span>Historial</span><span>Alertas</span><span>Reportes</span></div></div><div class="platform-body"><div class="platform-title"><div><small>MONITOREO / UNIDAD BIOCORE</small><h3>Vista general</h3></div><span class="illustrative-label">Vista ilustrativa de la interfaz</span></div><div class="platform-cards"><div class="platform-card status-card"><small>ESTADO DEL EQUIPO</small><strong><i></i> En seguimiento</strong><span>Unidad BIOCORE</span></div><div class="platform-card variable-card green"><small>TEMPERATURA</small><strong>—</strong><span>SHT31 · disponible</span><div class="mini-chart green-chart"><i></i><i></i><i></i><i></i><i></i><i></i></div></div><div class="platform-card variable-card amber"><small>HUMEDAD</small><strong>—</strong><span>SHT31 · disponible</span><div class="mini-chart amber-chart"><i></i><i></i><i></i><i></i><i></i><i></i></div></div><div class="platform-card variable-card blue"><small>SEÑAL ANTE GASES COMBUSTIBLES</small><strong>—</strong><span>MQ-2 · lectura relativa</span><div class="signal-line"></div></div></div><div class="platform-lower"><div class="history-panel"><div class="panel-heading"><strong>Historial</strong><span>Registros del proceso</span></div><div class="history-bars"><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></div><div class="axis"><span>Fecha</span><span>Origen</span><span>Estado</span></div></div><div class="events-panel"><div class="panel-heading"><strong>Eventos</strong><span>Seguimiento</span></div><div class="event-row"><i class="event-dot lime"></i><span>Registro disponible</span><small>Fecha y origen</small></div><div class="event-row"><i class="event-dot amber-dot"></i><span>Revisión sugerida</span><small>Contexto de campo</small></div><a href="#solicitar">Consultar propuesta <b>↗</b></a></div></div></div></div><div class="demo-badge">Vista ilustrativa</div></div></div>
    </section>

    <section class="access-section section-shell" aria-labelledby="access-title"><div class="section-kicker">Según el contexto</div><div class="split-heading"><h2 id="access-title">Cerca del equipo o desde BIOCORE.</h2><p>Dos caminos complementarios para que la información tenga sentido en el lugar donde se produce y en el lugar donde se decide.</p></div><div class="access-grid"><article class="access-card"><span class="access-icon">⌁</span><h3>Cerca del equipo</h3><p>El ESP32 podría emitir una red Wi-Fi y ofrecer una interfaz local ligera para configuración y consulta básica sin internet.</p><span class="future-label">Diseño de la próxima versión</span></article><article class="access-card accent-card"><span class="access-icon">↗</span><h3>Con conectividad externa</h3><p>Con una red disponible, el equipo podría sincronizar datos y permitir consultar el historial desde BIOCORE.</p><span class="future-label">Diseño de la próxima versión</span></article></div><div class="access-reminder"><strong>Importante</strong><span>El Wi-Fi local del ESP32 por sí solo no da acceso a la aplicación publicada en la nube.</span></div></section>

    <section class="impact-section" id="impacto" aria-labelledby="impact-title"><div class="section-shell"><div class="section-kicker light">Un mismo lenguaje de datos</div><div class="split-heading light-heading"><h2 id="impact-title">Tecnología que se adapta a quienes sostienen el proceso.</h2><p>BIOCORE busca conectar la experiencia cotidiana, la operación y la investigación sin reemplazar el conocimiento de quienes trabajan en campo.</p></div><div class="audience-grid">{#each audiences as audience, index}<article class="audience-card"><div class="audience-image"><img src={audience.image} alt={audience.alt} /><span>0{index + 1}</span></div><h3>{audience.title}</h3><p>{audience.text}</p><a href="#solicitar">Conocer la propuesta <span>↗</span></a></article>{/each}</div></div></section>

    <section class="request-section section-shell" id="solicitar" aria-labelledby="request-title"><div class="request-intro"><div class="section-kicker">Abramos la conversación</div><h2 id="request-title">Cuéntanos qué quieres observar.</h2><p>Cuanto más contexto tengamos, mejor podremos entender si BIOCORE puede acompañar tu instalación, comunidad o proyecto.</p><div class="contact-aside"><span>Contacto directo</span><a href="mailto:hola@biocore.org">hola@biocore.org</a><small>Canal editable para la siguiente etapa.</small></div></div><form class="request-form" on:submit={handleSubmit}><div class="form-row"><label>Nombre completo<input bind:value={formData.name} required type="text" placeholder="Tu nombre" /></label><label>Correo electrónico<input bind:value={formData.email} required type="email" placeholder="tu@correo.com" /></label></div><div class="form-row"><label>Teléfono<input bind:value={formData.phone} type="tel" placeholder="+00 000 000 000" /></label><label>Organización o comunidad<input bind:value={formData.organization} required type="text" placeholder="Nombre de la organización" /></label></div><div class="form-row"><label>Ubicación<input bind:value={formData.location} required type="text" placeholder="Municipio, país" /></label><label>Tipo y tamaño aproximado<select bind:value={formData.digester} required><option value="" disabled>Selecciona una opción</option><option>Artesanal · hasta 50 L</option><option>Experimental · 50 a 200 L</option><option>Rural · 200 a 500 L</option><option>Otro tamaño o tipo</option></select></label></div><label>Cuéntanos tu necesidad<textarea bind:value={formData.need} required rows="4" placeholder="¿Qué te gustaría monitorear o aprender?"></textarea></label><label class="consent"><input bind:checked={formData.consent} type="checkbox" /><span>Acepto que BIOCORE use estos datos para contactarme sobre una posible evaluación o piloto.</span></label><button class="button button-primary" type="submit">Enviar solicitud <span>↗</span></button>{#if formStatus !== 'idle'}<p class:form-error={formStatus === 'error'} class:form-success={formStatus === 'success'} class="form-feedback" role="status">{formMessage}</p>{/if}<small class="form-note">No hay una integración de correo o CRM conectada en esta versión. Este formulario muestra el flujo de solicitud y queda listo para conectarse a una ruta API.</small></form></section>

    <section class="faq-section" id="faq" aria-labelledby="faq-title"><div class="section-shell faq-layout"><div><div class="section-kicker">Preguntas frecuentes</div><h2 id="faq-title">Lo que conviene saber antes de empezar.</h2><p>Una propuesta clara también dice qué está en desarrollo.</p></div><div class="faq-list">{#each faqs as faq, index}<div class:active={openFaq === index} class="faq-item"><button type="button" aria-expanded={openFaq === index} on:click={() => (openFaq = openFaq === index ? -1 : index)}><span>{faq.question}</span><i>{openFaq === index ? '−' : '+'}</i></button>{#if openFaq === index}<div class="faq-answer"><p>{faq.answer}</p></div>{/if}</div>{/each}</div></div></section>
  </main>

  <footer class="site-footer"><div class="footer-main section-shell"><div class="footer-brand"><a class="brand brand-dark" href="/">BIOCORE<span>.</span></a><p>Tecnología para investigar y mejorar el aprovechamiento de residuos orgánicos.</p></div><div class="footer-links"><div><strong>Explorar</strong><a href="#solucion">Solución</a><a href="#kit">El kit</a><a href="#aplicacion">Aplicación</a></div><div><strong>Participar</strong><a href="#impacto">Impacto</a><a href="#solicitar">Solicitar BIOCORE</a><a href="/login">Iniciar sesión</a></div><div><strong>Contacto</strong><a href="mailto:hola@biocore.org">hola@biocore.org</a><a href="#faq">Preguntas frecuentes</a><a href="mailto:hola@biocore.org?subject=Aviso%20de%20privacidad">Aviso de privacidad</a></div></div></div><div class="footer-bottom section-shell"><span>© 2026 BIOCORE</span><span>Proyecto de grado desarrollado en la UNEG · Una propuesta abierta al aprendizaje de campo.</span></div></footer>
{/if}
