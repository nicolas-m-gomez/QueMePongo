---


---

<h1 id="quemepongo---segunda-iteración">QueMePongo - Segunda Iteración</h1>
<h2 id="generación-de-borradores-y-prendas">Generación de borradores y prendas</h2>
<p>Para generar prendas válidas las mismas se crearán a partir de los borradores.<br>
Como se requiere especificar el tipo de prenda en primer lugar, al generar un Borrador se deberá especificar en su creación el TipoPrenda. En caso de ser nulo se lanzará una excepción.</p>
<pre><code>class Borrador{
	...	
	public Borrador(TipoPrenda tipoPrenda){
		this.tipoPrenda = Objects.requireNonNull(tipoPrenda, "Debe especificar el tipo de prenda a cargar");
	}
}
</code></pre>
<p>Para especificar el resto de los atributos de una prenda se utilizarán métodos setters. Para el caso particular del Material se validará que sea consistente con el TipoPrenda.</p>
<pre><code>class Borrador{
	...	
	private Boolean tipoPrendaConsistenteCon(Material material) {		
		//Cada TipoPrenda tiene un listado de materiales que admite.
		//materialesAdecuados() retorna el listado de materiales del TipoPrenda
		return tipoPrenda.materialesAdecuados().anyMatch(m -&gt; m.equals(material)); 
	}
	void cargarMaterial(Material material) {
		if(!this.tipoPrendaConsistenteCon(material))
			throw new MaterialInconsistenteConTipoPrenda("El material no es consistente con el tipo de prenda")
		this.material = material;
	}
	...
}
</code></pre>
<p>Para generar la Prenda a partir del Borrador se deberá recurrir al método crearPrenda() en el cual se validará que se hayan cargado los atributos de la Prenda. En caso que la Trama sea nula se considerará como Lisa.</p>
<pre><code>class Borrador{
	...	
	public Prenda crearPrenda() {
		Objects.requireNonNull(this.material);
		Objects.requireNonNull(this.colorPrincipal);
		this.trama = (Objects.isNull(this.trama)) ? Trama.LISA : this.trama;
		return new Prenda(tipo, material, colorPrincipal, colorSecundario, trama);
	}
	...
}
</code></pre>
<h2 id="generación-de-uniformes">Generación de uniformes</h2>
<p>Para generar uniformes de diferentes colegios o instituciones privadas se implementará un factory method. La clase Sastre tendrá métodos abstractos para la creación de Prendas que serán definidas por sus implementaciones.</p>
<pre><code>abstract class Sastre {
	public Uniforme fabricarUniforme() {
		return new Uniforme(this.fabricarParteSuperior(), this.fabricarParteInferior() , this.fabricarCalzado());
	}	
	abstract Prenda fabricarParteSuperior();
	abstract Prenda fabricarParteInferior();
	abstract Prenda fabricarCalzado();
}
class SastreJohnson extends Sastre {
	@Override
	public Prenda fabricarParteSuperior() {
		Borrador borrador = new Borrador(TipoPrenda.CAMISA);
		borrador.cargarColorPrimario(Color.BLANCO);
		borrador.cargarMaterial(Material.ALGODON);
		return borrador.crearPrenda();
	}
	@Override
	public Prenda fabricarParteInferior() {
		Borrador borrador = new Borrador(TipoPrenda.PANTALON);
		borrador.cargarColorPrimario(Color.NEGRO);
		borrador.cargarMaterial(Material.DE_VESTIR);
		return borrador.crearPrenda();
	}
	@Override
	public Prenda fabricarCalzado() {
		Borrador borrador = new Borrador(TipoPrenda.ZAPATOS);
		borrador.cargarColorPrimario(Color.NEGRO);
		borrador.cargarMaterial(Material.CUERO);
		return borrador.crearPrenda();
	}
}
class SastreSanJuan extends Sastre{
	@Override
	public Prenda fabricarParteSuperior() {
		Borrador borrador = new Borrador(TipoPrenda.CHOMBA);
		borrador.cargarColorPrimario(Color.VERDE);
		borrador.cargarMaterial(Material.PIQUE);
		return borrador.crearPrenda();
	}
	@Override
	public Prenda fabricarParteInferior() {
		Borrador borrador = new Borrador(TipoPrenda.PANTALON);
		borrador.cargarColorPrimario(Color.GRIS);
		borrador.cargarMaterial(Material.ACETATO);
		return borrador.crearPrenda();
	}
	@Override
	public Prenda fabricarCalzado() {
		Borrador borrador = new Borrador(TipoPrenda.ZAPATILLAS);
		borrador.cargarColorPrimario(Color.BLANCO);
		borrador.cargarMaterial(Material.LONA);
		return borrador.crearPrenda();
	}
}
</code></pre>
<h2 id="diagrama-de-clases">Diagrama de Clases</h2>
<p><img src="https://raw.githubusercontent.com/nicolas-m-gomez/QueMePongo/master/DiagramaDeClases.png" alt="Diagrama de clases"></p>

