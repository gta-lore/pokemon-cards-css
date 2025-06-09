<script>
  import { onMount } from "svelte";

	import Search from "./Search.svelte";
	import CardList from "./Cards.svelte";
	import Card from "./lib/components/CardProxy.svelte";

	let showcase, basics, reverse, holos, cosmos, amazings, radiant, basicGallery, 
			vee, veeUltra, veeAlt, veeMax, veeMaxAlt, veeStar, 
			trainerHolo, rainbow, gold, veeGallery, shinyVault;

	let query = "";
	let isLoading = true;

	const getCards = async () => {
		let promiseArray = [];
		let cardFetch = await fetch("/data/cards.json");
		let cards = await cardFetch.json();
		return cards;
	};

	const loadCards = async() => {
		return getCards()
			.then((cards) => {
				window.cards = cards;
				showcase = cards[0];
				basics = cards.slice(1, 4);
				reverse = [...cards.slice(4, 7), ...cards.slice(70,76)];
				holos = cards.slice(7, 13);
				cosmos = cards.slice(13, 16);
				amazings = cards.slice(76, 85);
				radiant = cards.slice(16, 19);
				basicGallery = cards.slice(19, 22);
				vee = cards.slice(22, 25);
				veeUltra = cards.slice(25, 28);
				veeAlt = cards.slice(28, 34);
				veeMax = cards.slice(37, 40);
				veeMaxAlt = cards.slice(40, 43);
				veeStar = cards.slice(43, 46);
				trainerHolo = cards.slice(46, 52);
				rainbow = cards.slice(52, 58);
				gold = cards.slice(58, 64);
				veeGallery = cards.slice(64, 70);
				shinyVault = cards.slice(85,91);
				isLoading = false;
			});
	};

	onMount(() => {
		loadCards();
		const $headings = document.querySelectorAll("h1,h2,h3");
		const $anchor = [...$headings].filter((el) => {
			const id = el.getAttribute("id")?.replace(/^.*?-/g,"");
			const hash = window.location.hash?.replace(/^.*?-/g,"")
			return id === hash;
		})[0];
		if( $anchor ) {
			setTimeout(() => {
				$anchor.scrollIntoView();
			},100);
		}
	});
</script>

<main>
	<header>
		<h1 id="⚓-top">Cartas Pokémon <sup>V2</sup></h1>

		<p class="author">Por <a href="https://twitter.com/simeydotme"><svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>Twitter</title><path d="M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z"/></svg> @simeydotme</a> |
			<em><a href="https://github.com/simeydotme/pokemon-cards-css"><svg role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>GitHub</title><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg> Simon Goellner</em>
		</p>

		<section class="intro" id="⚓-intro">
			<p>
				Una colección de estilos <mark>CSS avanzados</mark> para crear
				efectos <mark>realistas</mark> en las caras de las cartas Pokémon. 
				Las cartas usan <mark>transformaciones 3D</mark>, <mark>filtros</mark>, <mark>modos de mezcla</mark>,
				<mark>degradados CSS</mark> e interacciones para brindar una experiencia única al mirarlas de cerca.
			</p>
		</section>

		<div class="showcase">
			{#if !showcase}
				cargando...
			{:else}
				<Card
					id={showcase.id}
					name={showcase.name}
					set={showcase.set}
					number={showcase.number}
					types={showcase.types}
					supertype={showcase.supertype}
					subtypes={showcase.subtypes}
					rarity={showcase.rarity}
					isReverse={showcase.isReverse}
					showcase={true}
				/>
			{/if}
		</div>

		<section class="info">
			<h2>¡Haz clic en una carta para verla de cerca!</h2>

			<hr />

			<p class="small">
				Estoy usando SvelteJS para manejar la interactividad y el estado; <strong>
					asignando valores a propiedades CSS personalizadas</strong> (variables) que
					a su vez controlan los efectos y transformaciones 3D. 
					<br>
					<a href="https://github.com/simeydotme/pokemon-cards-css">El código fuente está en el repositorio</a>.
			</p>
		</section>
	</header>

	<Search bind:query />

	{#if query.length < 3}

		<h2 id="⚓-common">
			<a href="#⚓-common">
				Comunes y poco comunes
			</a>
		</h2>
		<p>
			Todas las cartas obtienen una rotación 3D con CSS basada en la posición del cursor.<br /> Las cartas básicas
			no holográficas simplemente aplican un <mark>efecto de destello/resplandor</mark> que sigue al mouse.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each basics as card, index}
					<Card
						id={card.id}
						name={card.name}
						img={card.images.large}
						number={card.number}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-reverse">
			<a href="#⚓-reverse">
				Holo reverso no raras
			</a>
		</h2>
		<p>
			Las cartas holo reverso vienen en <mark>muchas formas y tamaños</mark> (entrenador, etapa1, y diferentes tipos de energía).<br /> Por lo tanto
			hay algunos ejemplos aquí para mostrar las diferentes variaciones. El <mark>fondo usa una lámina y una capa de máscara</mark>
			junto con un resplandor. También <mark>recorto el resplandor</mark> en la ventana de la imagen para tratar la imagen y el holofoil de manera diferente.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each reverse as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
						isReverse
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-holo">
			<a href="#⚓-holo">
				Holo raras
			</a>
		</h2>
		<p>
			Las cartas holo tienen un <mark>efecto holo de haz vertical adicional</mark>.<br /> Esto usa una
			combinación de <mark>degradados y filtros repetidos</mark>, con <mark>clip-path</mark> para enmascarar
			las áreas holo para cada etapa. Para que el efecto holo cambie al rotar la carta, establezco la
			posición de fondo de cada capa de degradado basada en el cursor.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each holos as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-galaxy">
			<a href="#⚓-galaxy">
				Holo Galaxia/Cosmos
			</a>
		</h2>
		<p>
			Similar al Holo, pero usa un <mark>fondo de imagen especial de efecto galaxia</mark>
			con un <mark>degradado arcoíris configurado para color-dodge y color-burn</mark> encima.
		</p>
		<h3>¡Un clásico instantáneo para cualquier fan de PTCG!</h3>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each cosmos as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-amazing">
			<a href="#⚓-amazing">
				Holo Raras Increíbles
			</a>
		</h2>
		<p>
			Las cartas Raras Increíbles tienen un <mark>foil brillante muy único</mark> que se extiende más allá del marco y es mucho más brillante que
			un efecto holo regular, y texturizado. Logramos esto usando una máscara y aplicando un efecto de brillo con un filtro de aclarado.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each amazings as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-radiant">
			<a href="#⚓-radiant">
				Holo Radiante
			</a>
		</h2>
		<p>
			¡El holofoil más nuevo añadido a la serie!<br /> El efecto radiante resultó
			<mark>muy difícil de emular</mark>
			sin matemáticas locas, así que opté por un <mark>patrón de degradado lineal entrecruzado</mark> que se mueve
			a través de la carta.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each radiant as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-trainer-gallery-holo">
			<a href="#⚓-trainer-gallery-holo">
				Holo Galería de Entrenadores
			</a>
		</h2>
		<p>
			El efecto para los holofoils de la Galería de Entrenadores les da un <mark>efecto metálico con brillo iridiscente</mark>. Esto se logra con un <mark>gran degradado lineal de color-dodge</mark>, y un
			<mark>degradado radial de hard-light</mark> encima, en la posición del cursor, para dar el brillo.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each basicGallery as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-v">
			<a href="#⚓-v">
				Pokémon V
			</a>
		</h2>
		<p>
			Las cartas V tienen un <mark>efecto holográfico diagonal</mark> que parece viajar en direcciones
			opuestas cuando inclinas la carta hacia la luz.
		</p>
		<p>
			Este efecto se logra con <mark>múltiples degradados de fondo</mark> y cambio las posiciones de fondo
			basadas en la x/y del cursor. Los degradados están configurados en color-dodge, y hay un sutil efecto
			de ruido svg.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each vee as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-v-full-art">
			<a href="#⚓-v-full-art">
				Pokémon V <sup>(Arte Completo)</sup>
			</a>
		</h2>
		<p>
			Similar al efecto Pokémon V, las cartas de Arte Completo <mark>usan degradados diagonales</mark>, pero tienen
			<mark>textura adicional</mark> cuando se miran desde ciertos ángulos. Esto se logra con
			una <mark>imagen de fondo adicional con un filtro de exclusión</mark>. El efecto también es más
			vibrante lo que añade al aspecto metálico.
		</p>
		<p>
			El efecto de textura no es idéntico a la realidad ya que las cartas reales tienen cada una un patrón único que
			sigue el arte.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each veeUltra as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-v-alternate-art">
			<a href="#⚓-v-alternate-art">
				Pokémon V <sup>(Arte Alternativo)</sup>
			</a>
		</h2>
		<p>
			Las cartas Pokémon V de Arte Alternativo tienen <mark
				>prácticamente el mismo efecto holo que las cartas Ultra Raras (Arte Completo)</mark
			>. La única diferencia es la textura del patrón.
		</p>
		<p>El efecto se ve algo diferente debido al tipo de ilustración.</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each veeAlt as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-v-max">
			<a href="#⚓-v-max">
				VMax
			</a>
		</h2>
		<p>
			El efecto de degradado de Pokémon VMax es más sutil, usando un <mark
				>degradado de fondo más grande</mark
			>
			que se mueve más lentamente. Pero el <mark>efecto de textura es más pronunciado</mark>.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each veeMax as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-v-max-alternate">
			<a href="#⚓-v-max-alternate">
				VMax <sup>(Alternativo/Arcoíris)</sup>
			</a>
		</h2>
		<p>
			Hay algunas cartas VMax que muestran un <mark>arte completo o alternativo</mark>. Estas son
			<mark>clasificadas como "raras arcoíris"</mark>
			y tienen un efecto similar a las cartas arcoíris. Es una
			<mark>superposición vibrante y brillante</mark>.
		</p>
		<p>
			Esto se logra con una <mark>imagen de fondo de brillos/brillantinas</mark>, y una imagen de fondo de patrón de textura,
			intercalando los degradados lineales habituales.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each veeMaxAlt as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-v-star">
			<a href="#⚓-v-star">
				VStar
			</a>
		</h2>
		<p>
			De nuevo con los <mark>degradados diagonales superponiendo una textura</mark>, las VStar son bastante
			<mark>similares a las Ultra Raras</mark>
			(Arte Completo/Alt). Las cartas son generalmente <mark>más brillantes con un tono pastel</mark>, lo que
			hace que el degradado y la textura sean más sutiles.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each veeStar as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-trainer-full-art">
			<a href="#⚓-trainer-full-art">
				Holo Entrenador <sup>(Arte Completo / Galería de Entrenadores)</sup>
			</a>
		</h2>
		<p>
			De nuevo con los <mark>degradados diagonales superponiendo una textura</mark>, las VStar son bastante
			<mark>similares a las Ultra Raras</mark>
			(Arte Completo/Alt). Las cartas son generalmente <mark>más brillantes con un tono pastel</mark>, lo que
			hace que el degradado y la textura sean más sutiles.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each trainerHolo as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-rainbow-rare">
			<a href="#⚓-rainbow-rare">
				Raras Arcoíris <sup>(VMax, VStar)</sup>
			</a>
		</h2>
		<p>
			Las Raras Arcoíris tienen un <mark>efecto súper brillante sobre degradados pastel</mark>. Esto se
			logra con una imagen de fondo de brillantinas y un
			<mark>fondo de mezcla color-burn/hard-light</mark> para algunos degradados pastel.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each rainbow as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-secret-rare">
			<a href="#⚓-secret-rare">
				Raras Secretas <sup>(Oro)</sup>
			</a>
		</h2>
		<p>
			<mark>¡ORO!</mark> Aquí aplicamos dos capas de brillo una encima de la otra con un efecto de superposición y
		<mark>deslizamos las dos capas en direcciones opuestas</mark>. También <mark>enmascaramos la imagen foil</mark> con un degradado para que
		las capas foil y brillo sean mutuamente excluyentes. ¡El efecto resultante es una capa de brillo centelleante!
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each gold as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-trainer-gallery-v">
			<a href="#⚓-trainer-gallery-v">
				Galería de Entrenadores <sup>(V / VMax)</sup>
			</a>
		</h2>
		<p>
			Las cartas V y VMax en la Galería de Entrenadores son generalmente bastante similares a las cartas V y VMax normales,
			así que aquí solo ajusté un poco los valores y añadí una textura de fondo diferente.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each veeGallery as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

		<h2 id="⚓-shiny-vault">
			<a href="#⚓-shiny-vault">
				Bóveda Shiny <sup>(Básicas / Etapa 1 / V / VMax)</sup>
			</a>
		</h2>
		<p>
			Las cartas de la Bóveda Shiny tienen un efecto bastante único en el que el fondo foil es de un color plateado brillante.
			Para lograr esto aplicamos la imagen foil con algunos degradados radiales para oscurecer el foil
			sobre el fondo. Esto crea un efecto ligeramente plateado sobre el fondo blanco de la carta.
			Este efecto funciona mejor en Firefox.
		</p>

		<CardList>
			{#if isLoading}
				cargando...
			{:else}
				{#each shinyVault as card, index}
					<Card
						id={card.id}
						name={card.name}
						number={card.number}
						set={card.set}
						types={card.types}
						supertype={card.supertype}
						subtypes={card.subtypes}
						rarity={card.rarity}
					/>
				{/each}
			{/if}
		</CardList>

	{/if}
</main>

<div class="back-to-top">
  <a href="#⚓-top">Volver al inicio</a>
</div>

<style>
  .back-to-top a {
    color: inherit;
    text-decoration: none;
		z-index: 999;
  }
</style>
