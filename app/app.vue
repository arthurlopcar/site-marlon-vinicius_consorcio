<script setup lang="ts">
const menuOpen = ref(false)
const currentYear = new Date().getFullYear()
let revealObserver: IntersectionObserver | undefined
const whatsappNumber = '5531996907206'
const defaultWhatsAppMessage = encodeURIComponent(
  'Olá, Marlon! Encontrei seu site e gostaria de conversar sobre consórcio.',
)
const whatsappLink = `https://wa.me/${whatsappNumber}?text=${defaultWhatsAppMessage}`
const lead = reactive({
  name: '',
  objective: 'Imóvel',
  creditValue: 300000,
  monthlyBudget: '',
  city: '',
})

useHead({
  htmlAttrs: { lang: 'pt-BR' },
  title: 'Marlon Vinicius | Consórcio e Patrimônio',
  meta: [
    {
      name: 'description',
      content:
        'Planeje a conquista do seu imóvel ou veículo com Marlon Vinicius, consultor autorizado Ademicon. Encontre um plano de consórcio que faça sentido para você.',
    },
    { name: 'theme-color', content: '#102a24' },
  ],
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Manrope:wght@400;500;600;700;800&display=swap',
    },
  ],
})

onMounted(() => {
  const revealTargets = document.querySelectorAll<HTMLElement>(
    '.intro-content, .section-heading, .solution-card, .process-heading, .process-step, .simulator-copy, .simulator-form, .regions-inner > div, .about-photo, .about-copy, .faq-heading, .faq-item, .closing-inner',
  )

  if (!('IntersectionObserver' in window)) {
    return
  }

  document.documentElement.classList.add('motion-enabled')

  revealTargets.forEach((element) => {
    element.classList.add('motion-reveal')
  })

  revealObserver = new IntersectionObserver(
    (entries, observer) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible')
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.14, rootMargin: '0px 0px -35px 0px' },
  )

  revealTargets.forEach((element) => revealObserver?.observe(element))
})

onBeforeUnmount(() => {
  revealObserver?.disconnect()
  document.documentElement.classList.remove('motion-enabled')
})

function closeMenu() {
  menuOpen.value = false
}

function sendLeadToWhatsApp() {
  const formattedCredit = new Intl.NumberFormat('pt-BR', {
    style: 'currency',
    currency: 'BRL',
    maximumFractionDigits: 0,
  }).format(lead.creditValue)
  const message = [
    'Olá, Marlon! Preenchi a pré-simulação no site.',
    `Nome: ${lead.name.trim()}`,
    `Objetivo: ${lead.objective}`,
    `Crédito desejado: ${formattedCredit}`,
    lead.monthlyBudget
      ? `Parcela mensal que pretendo: ${new Intl.NumberFormat('pt-BR', {
          style: 'currency',
          currency: 'BRL',
        }).format(Number(lead.monthlyBudget))}`
      : '',
    lead.city.trim() ? `Cidade/UF: ${lead.city.trim()}` : '',
  ]
    .filter(Boolean)
    .join('\n')

  window.location.assign(
    `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`,
  )
}
</script>

<template>
  <NuxtRouteAnnouncer />
  <header class="site-header">
    <div class="container header-inner">
      <a class="brand" href="#inicio" aria-label="Marlon Vinicius — início">
        <span class="brand-mark" aria-hidden="true">M</span>
        <span class="brand-copy">
          <strong>Marlon Vinicius</strong>
          <span>CONSÓRCIO E PATRIMÔNIO</span>
        </span>
      </a>

      <button
        class="menu-toggle"
        type="button"
        :aria-label="menuOpen ? 'Fechar menu' : 'Abrir menu'"
        :aria-expanded="menuOpen"
        aria-controls="primary-navigation"
        @click="menuOpen = !menuOpen"
      >
        <span></span><span></span>
      </button>

      <nav
        id="primary-navigation"
        class="main-nav"
        :class="{ 'is-open': menuOpen }"
        aria-label="Navegação principal"
      >
        <a href="#solucoes" @click="closeMenu">Soluções</a>
        <a href="#simulador" @click="closeMenu">Simulador</a>
        <a href="#como-funciona" @click="closeMenu">Como funciona</a>
        <a href="#sobre" @click="closeMenu">Sobre mim</a>
        <a href="#duvidas" @click="closeMenu">Dúvidas</a>
      </nav>
      <a
        class="button button-small button-dark header-cta"
        :href="whatsappLink"
        target="_blank"
        rel="noreferrer"
      >
        Vamos conversar <span aria-hidden="true">↗</span>
      </a>
    </div>
  </header>

  <main>
    <section id="inicio" class="hero">
      <div class="hero-texture" aria-hidden="true"></div>
      <div class="container hero-grid">
        <div class="hero-copy">
          <p class="eyebrow eyebrow-light"><span class="status-dot"></span> CONSULTORIA EM CONSÓRCIO</p>
          <h1>Seu próximo passo pode ser o começo de um <em>grande patrimônio.</em></h1>
          <p class="hero-description">
            Transforme seus planos em uma estratégia possível. Eu te ajudo a entender
            as opções e encontrar um caminho que combine com o seu momento.
          </p>
          <div class="hero-actions">
            <a
              class="button button-accent"
              href="#simulador"
            >
              Descubra seu plano <span aria-hidden="true">↗</span>
            </a>
            <a class="text-link text-link-light" href="#solucoes">
              Conheça as soluções <span aria-hidden="true">↓</span>
            </a>
          </div>
          <div class="hero-proof">
            <div class="proof-avatars" aria-hidden="true">
              <span>MV</span><span>✳</span><span>@</span>
            </div>
            <p><strong>Um plano com a sua cara</strong><br />Atendimento próximo, do primeiro papo à conquista.</p>
          </div>
        </div>

        <div class="hero-visual">
          <div class="hero-image">
            <img
              src="https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?auto=format&fit=crop&w=1200&q=85"
              alt="Casa contemporânea cercada por um jardim"
              fetchpriority="high"
            />
            <div class="image-shade"></div>
            <div class="image-caption">
              <span class="caption-icon" aria-hidden="true">↗</span>
              <span>Planejamento de hoje.<br /><strong>Conquistas de amanhã.</strong></span>
            </div>
          </div>
          <div class="floating-note">
            <span class="note-icon" aria-hidden="true">✳</span>
            <span><strong>Seu objetivo</strong><br />merece um plano.</span>
          </div>
          <span class="hero-sparkle sparkle-one" aria-hidden="true">✳</span>
          <span class="hero-sparkle sparkle-two" aria-hidden="true">✳</span>
        </div>
      </div>
      <div class="hero-bottom container">
        <span>UM BOM PLANO COMEÇA COM UMA BOA CONVERSA</span>
        <a href="#solucoes" aria-label="Descer para conhecer as soluções">↓</a>
        <span>IMÓVEIS&nbsp;&nbsp; · &nbsp;&nbsp;VEÍCULOS&nbsp;&nbsp; · &nbsp;&nbsp;PLANEJAMENTO</span>
      </div>
    </section>

    <section class="intro-section section-pad">
      <div class="container intro-grid">
        <div class="section-kicker">
          <span class="eyebrow">PATRIMÔNIO SE CONSTRÓI</span>
          <span class="kicker-line"></span>
          <span class="kicker-number">01 / 04</span>
        </div>
        <div class="intro-content">
          <h2>Não é só sobre comprar.<br /><span>É sobre escolher bem.</span></h2>
          <div class="intro-bottom">
            <p>
              Um imóvel, um carro, mais tranquilidade para o futuro. Cada conquista
              começa de um jeito diferente — e o melhor plano é aquele que respeita
              seus objetivos e a sua realidade.
            </p>
            <a class="text-link" href="#como-funciona">
              Entenda como posso ajudar <span aria-hidden="true">↗</span>
            </a>
          </div>
        </div>
      </div>
    </section>

    <section id="solucoes" class="solutions-section section-pad">
      <div class="container">
        <div class="section-heading">
          <div>
            <p class="eyebrow">POSSIBILIDADES PARA VOCÊ</p>
            <h2>Qual conquista está<br />nos seus planos?</h2>
          </div>
          <p class="section-heading-aside">
            A gente conversa sobre o que você quer realizar e avalia juntos as
            possibilidades de consórcio para o seu momento.
          </p>
        </div>

        <div class="solution-grid">
          <article class="solution-card solution-card-home">
            <div class="solution-image">
              <img
                src="https://images.unsplash.com/photo-1600596542815-ffad4c1539a9?auto=format&fit=crop&w=1000&q=80"
                alt="Residência moderna com linhas arquitetônicas contemporâneas"
                loading="lazy"
              />
              <span class="card-number">01</span>
            </div>
            <div class="solution-info">
              <div><span class="card-label">SEU LUGAR NO MUNDO</span><h3>Imóveis</h3></div>
              <a
                :href="whatsappLink"
                target="_blank"
                rel="noreferrer"
                aria-label="Conversar sobre consórcio de imóveis"
              >↗</a>
            </div>
            <p>Planeje a compra, construção ou reforma do seu próximo imóvel.</p>
          </article>

          <article class="solution-card solution-card-car">
            <div class="solution-image">
              <img
                src="https://images.unsplash.com/photo-1503376780353-7e6692767b70?auto=format&fit=crop&w=1000&q=80"
                alt="Carro esportivo estacionado em uma estrada"
                loading="lazy"
              />
              <span class="card-number">02</span>
            </div>
            <div class="solution-info">
              <div><span class="card-label">LIBERDADE DE IR MAIS LONGE</span><h3>Veículos</h3></div>
              <a
                :href="whatsappLink"
                target="_blank"
                rel="noreferrer"
                aria-label="Conversar sobre consórcio de veículos"
              >↗</a>
            </div>
            <p>Organize seus planos para trocar ou conquistar seu veículo.</p>
          </article>

          <article class="solution-card solution-card-plan">
            <div class="plan-art" aria-hidden="true">
              <span class="plan-orbit orbit-a"></span>
              <span class="plan-orbit orbit-b"></span>
              <span class="plan-star">✳</span>
              <span class="plan-art-label">TEMPO + ESTRATÉGIA</span>
              <span class="plan-art-number">∞</span>
            </div>
            <div class="solution-info">
              <div><span class="card-label">OLHAR PARA O FUTURO</span><h3>Planejamento</h3></div>
              <a
                :href="whatsappLink"
                target="_blank"
                rel="noreferrer"
                aria-label="Conversar sobre planejamento patrimonial"
              >↗</a>
            </div>
            <p>Conheça uma forma planejada de se preparar para seus próximos passos.</p>
          </article>
        </div>
        <p class="section-footnote">
          Consórcio é uma modalidade de compra baseada na união de pessoas em grupo,
          com regras previstas em contrato.
        </p>
      </div>
    </section>

    <section id="simulador" class="simulator-section section-pad">
      <div class="container simulator-grid">
        <div class="simulator-copy">
          <p class="eyebrow">UM PRIMEIRO PASSO, SEM COMPLICAÇÃO</p>
          <h2>Vamos entender<br />o plano que cabe<br /><em>na sua vida.</em></h2>
          <p>
            Conte um pouco sobre o seu objetivo. As informações seguem direto para
            uma conversa com o Marlon no WhatsApp.
          </p>
          <div class="simulator-note">
            <span aria-hidden="true">i</span>
            <p>
              Isto não é uma cotação nem uma promessa de parcela ou contemplação.
              Valores, prazos e condições dependem do grupo e da análise da proposta.
            </p>
          </div>
        </div>

        <form class="simulator-form" @submit.prevent="sendLeadToWhatsApp">
          <div class="form-heading">
            <span class="form-step">PRÉ-SIMULAÇÃO · 01</span>
            <span class="form-decoration" aria-hidden="true">✳</span>
          </div>
          <label class="form-field">
            <span>Como podemos te chamar?</span>
            <input
              v-model="lead.name"
              type="text"
              name="name"
              autocomplete="name"
              placeholder="Seu nome"
              required
              maxlength="80"
            />
          </label>
          <div class="form-row">
            <label class="form-field">
              <span>O que você quer planejar?</span>
              <select v-model="lead.objective" name="objective" required>
                <option>Imóvel</option>
                <option>Veículo</option>
                <option>Planejamento patrimonial</option>
                <option>Outro objetivo</option>
              </select>
            </label>
            <label class="form-field">
              <span>Crédito desejado</span>
              <div class="input-prefix">
                <span>R$</span>
                <input
                  v-model.number="lead.creditValue"
                  type="number"
                  name="creditValue"
                  min="50000"
                  max="5000000"
                  step="10000"
                  inputmode="numeric"
                  required
                />
              </div>
            </label>
          </div>
          <div class="form-row">
            <label class="form-field">
              <span>Parcela mensal desejada <small>(opcional)</small></span>
              <div class="input-prefix">
                <span>R$</span>
                <input
                  v-model="lead.monthlyBudget"
                  type="number"
                  name="monthlyBudget"
                  min="0.01"
                  max="100000"
                  step="any"
                  inputmode="numeric"
                  placeholder="Ex.: 1.500"
                />
              </div>
            </label>
            <label class="form-field">
              <span>Sua cidade <small>(opcional)</small></span>
              <input
                v-model="lead.city"
                type="text"
                name="city"
                autocomplete="address-level2"
                placeholder="Cidade / UF"
                maxlength="80"
              />
            </label>
          </div>
          <button class="button button-accent form-submit" type="submit">
            Continuar pelo WhatsApp <span aria-hidden="true">↗</span>
          </button>
          <p class="form-privacy">
            Ao continuar, você será direcionado para uma conversa com o Marlon. Seus dados não são
            armazenados por este site.
          </p>
        </form>
      </div>
    </section>

    <section class="regions-section">
      <div class="container regions-inner">
        <div>
          <p class="eyebrow">PERTO DE VOCÊ, ONDE VOCÊ ESTIVER</p>
          <h2>Atendimento em diferentes regiões.</h2>
        </div>
        <div class="region-list" aria-label="Regiões de atendimento">
          <span>Ipatinga <strong>MG</strong></span>
          <span>João Monlevade <strong>MG</strong></span>
          <span>São Paulo <strong>SP</strong></span>
          <span class="region-more">E outras regiões</span>
        </div>
      </div>
    </section>

    <section id="como-funciona" class="process-section section-pad">
      <div class="container process-grid">
        <div class="process-heading">
          <p class="eyebrow eyebrow-light">SEM COMPLICAÇÃO</p>
          <h2>Do primeiro papo<br />ao seu próximo <em>passo.</em></h2>
          <p>Você não precisa chegar com tudo decidido. A gente organiza as ideias juntos.</p>
          <a
            class="button button-outline"
            :href="whatsappLink"
            target="_blank"
            rel="noreferrer"
          >
            Começar uma conversa <span aria-hidden="true">↗</span>
          </a>
        </div>
        <div class="process-steps">
          <article class="process-step">
            <span class="step-index">01</span>
            <div><h3>Me conta o que você quer</h3><p>Objetivos, prazo e o que faz sentido para a sua vida hoje.</p></div>
            <span class="step-arrow" aria-hidden="true">↘</span>
          </article>
          <article class="process-step">
            <span class="step-index">02</span>
            <div><h3>Avaliamos as possibilidades</h3><p>Uma conversa clara para entender opções, valores e condições.</p></div>
            <span class="step-arrow" aria-hidden="true">↘</span>
          </article>
          <article class="process-step">
            <span class="step-index">03</span>
            <div><h3>Você decide com confiança</h3><p>Com as informações em mãos, você escolhe se quer seguir.</p></div>
            <span class="step-arrow" aria-hidden="true">↗</span>
          </article>
        </div>
      </div>
    </section>

    <section id="sobre" class="about-section section-pad">
      <div class="container about-grid">
        <div class="about-photo">
          <img
            src="https://images.unsplash.com/photo-1497366754035-f200968a6e72?auto=format&fit=crop&w=1000&q=85"
            alt="Ambiente de trabalho acolhedor para conversas de planejamento"
            loading="lazy"
          />
          <span class="about-photo-tag">ATENDIMENTO PRÓXIMO · TRANSPARENTE</span>
        </div>
        <div class="about-copy">
          <p class="eyebrow">PRAZER, MARLON</p>
          <h2>Seu plano começa<br />com uma conversa <em>de verdade.</em></h2>
          <p>
            Acredito que construir patrimônio não precisa ser um caminho solitário
            ou cheio de dúvidas. Meu papel é ouvir seus planos, explicar as opções
            com clareza e te acompanhar na escolha.
          </p>
          <div class="authorization">
            <span class="authorization-mark" aria-hidden="true">✓</span>
            <span>
              <strong>Consultor autorizado Ademicon</strong><br />
              Consórcio com orientação próxima e transparente.
            </span>
          </div>
          <a
            class="text-link"
            :href="whatsappLink"
            target="_blank"
            rel="noreferrer"
          >
            Me encontre no Instagram <span aria-hidden="true">↗</span>
          </a>
        </div>
      </div>
    </section>

    <section id="duvidas" class="faq-section section-pad">
      <div class="container faq-grid">
        <div class="faq-heading">
          <p class="eyebrow">INFORMAÇÃO TAMBÉM É PLANEJAMENTO</p>
          <h2>Dúvidas que<br />podem ser suas.</h2>
          <p>Ficou com outra pergunta? Me chama e conversamos sem compromisso.</p>
          <a
            class="text-link"
            :href="whatsappLink"
            target="_blank"
            rel="noreferrer"
          >
            Pergunte pelo Instagram <span aria-hidden="true">↗</span>
          </a>
        </div>
        <div class="faq-list">
          <details class="faq-item">
            <summary>O que é consórcio?<span aria-hidden="true">+</span></summary>
            <p>
              É uma modalidade de compra planejada em grupo. Os participantes contribuem
              mensalmente e concorrem às contemplações conforme as regras do grupo e do contrato.
            </p>
          </details>
          <details class="faq-item">
            <summary>Como acontece a contemplação?<span aria-hidden="true">+</span></summary>
            <p>
              A contemplação ocorre por sorteio ou lance, de acordo com as regras do grupo.
              Não há garantia de contemplação em um prazo específico; isso depende das
              assembleias e das condições contratuais.
            </p>
          </details>
          <details class="faq-item">
            <summary>Consórcio é financiamento?<span aria-hidden="true">+</span></summary>
            <p>
              Não. São modalidades diferentes, com custos, prazos e formas de acesso ao
              crédito próprios. Antes de decidir, é importante analisar as condições do
              contrato e o que funciona para você.
            </p>
          </details>
          <details class="faq-item">
            <summary>Posso usar consórcio para imóveis ou veículos?<span aria-hidden="true">+</span></summary>
            <p>
              Há grupos voltados a diferentes objetivos, como imóveis e veículos. As
              possibilidades de uso e os critérios dependem das regras da categoria e do contrato.
            </p>
          </details>
        </div>
      </div>
    </section>

    <section class="closing-section">
      <div class="container closing-inner">
        <p class="eyebrow eyebrow-light">O SEU PRÓXIMO CAPÍTULO</p>
        <h2>Vamos transformar<br />seus planos em <em>possibilidades?</em></h2>
        <a
          class="button button-accent"
          :href="whatsappLink"
          target="_blank"
          rel="noreferrer"
        >
          Fale comigo no Instagram <span aria-hidden="true">↗</span>
        </a>
        <span class="closing-decoration" aria-hidden="true">✳</span>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <div class="container footer-main">
      <a class="brand brand-footer" href="#inicio" aria-label="Marlon Vinicius — voltar ao início">
        <span class="brand-mark" aria-hidden="true">M</span>
        <span class="brand-copy">
          <strong>Marlon Vinicius</strong>
          <span>CONSÓRCIO E PATRIMÔNIO</span>
        </span>
      </a>
      <p>Planejamento para chegar mais perto do que importa.</p>
      <a
        class="footer-social"
        href="https://www.instagram.com/marlonvini1/"
        target="_blank"
        rel="noreferrer"
      >
        @marlonvini1 <span aria-hidden="true">↗</span>
      </a>
    </div>
    <div class="container footer-legal">
      <p>© {{ currentYear }} Marlon Vinicius. Todos os direitos reservados.</p>
      <p>
        As condições, taxas, prazos e regras de contemplação são definidas no contrato
        do grupo. A contemplação não é imediata nem garantida em prazo determinado.
      </p>
    </div>
  </footer>
</template>
