---


---

<h1 id="quemepongo---primera-iteración">QueMePongo - Primera Iteración</h1>
<h2 id="especificar-tipo-de-prenda-y-categoría">Especificar tipo de Prenda y Categoría</h2>
<p>Para circunscribir el universo de valores, elijo modelar Categoría y TipoPrenda con enums.<br>
Además. para evitar que haya prendas cuya categoría no se condiga con su tipo, cada instacia de TipoPrenda contendrá a qué Categoría pertenece.</p>
<pre><code>enum Categoria{
	PARTE_SUPERIOR
	PARTE_INFERIOR
	CALZADO
	ACCESORIO
}
enum TipoPrenda{
	CAMISA(Categoria.PARTE_SUPERIOR),
	REMERA(Categoria.PARTE_SUPERIOR),
	PANTALON(Categoria.PARTE_INFERIOR),
	ZAPATOS(Categoria.CALZADO),
	AROS(Categoria.ACCESORIO);
	private Categoria categoria;
	//Constructor TipoPrenda
	public TipoPrenda(Categoria categoria){
		this.categoria = categoria;
	}
	Categoria categoria(
		return categoria;
	)
}
</code></pre>
<h2 id="crear-una-prenda">Crear una prenda</h2>
<p>Como mínimo, una prenda debe tener Color Primario, Tipo, Material y Categoría. Para modelar Material también utilizo enums ya que no tienen comportamiento. Para Color utilizamos una clase con tres int que representan la codificación RGB.<br>
Para cumplir este requerimiento la clase Prenda tiene dos constructores:</p>
<ul>
<li>Uno con Color Primario, Tipo y Material</li>
<li>Otro con los datos anteriores y Color Secundario</li>
</ul>
<p>En ambos casos se requere como no núlos Color Primario, Tipo y Material. Caso contrario se lanza una excepción.</p>
<pre><code>public class Prenda {
	private TipoPrenda tipo;
	private Material material;
	private Color colorPrincipal;
	private Color colorSecundario;
	//Constructor con datos mínimos solicitados (Color Principal, Material y Tipo de Prenda)
	public Prenda(TipoPrenda tipo, Material material, Color colorPrincipal) {
		Object.requireNonNull(this.tipo, "Tipo de Prenda no puede ser núlo") = tipo;
		Object.requireNonNull(this.material, "Material no puede ser núlo") = material;
		Object.requireNonNull(this.colorPrincipal, "Color Principal no puede ser núlo") = colorPrincipal;
	}
	//Constructor con datos Color Secundario agregado   	
	public Prenda(TipoPrenda tipo, Material material, Color colorPrincipal, Color colorSecundario) {
		Object.requireNonNull(this.tipo, "Tipo de Prenda no puede ser núlo") = tipo;
		Object.requireNonNull(this.material, "Material no puede ser núlo") = material;
		Object.requireNonNull(this.colorPrincipal, "Color Principal no puede ser núlo") = colorPrincipal;
		this.colorSecundario = colorSecundario;
	}
	//Para idenficiar a qué categoría pertenece una Prenda
	public Categoria categoria(){
		return this.tipo.categoria();
	}
}
</code></pre>
<h2 id="diagrama-de-clases">Diagrama de Clases</h2>
<p><img src="https://raw.githubusercontent.com/nicolas-m-gomez/QueMePongo/master/DiagramaDeClases.png" alt="Diagrama de clases"></p>

