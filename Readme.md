# QueMePongo - Cuarta Iteración
## Consulta del clima
Para obtener el clima de la API de AccuWeather sin gastar de más generé el adapter CondicionClimatica con un parámetro que limita la cantidad de consultas por hora que se pueden realizar a la API. Si se excede el límite se devuelve a la consulta el último dato que se obtuvo de la API.
```
public class CondicionClimaticaImpl implements CondicionClimatica{
	...
	private static int INTERVALO_HH_CONSULTAS = 3;
	
	public CondicionClimaticaImpl(Ciudad ciudad) {
		this.ciudad = ciudad;
		this.obtenerDatosApi();
	}
	
	@Override
	public Double obtenerTemperaturaActual() {
		UtilitarioFechas utilitarioFechas = new UtilitarioFechas();
		if(utilitarioFechas.diferenciaEnHHEntre(this.fechaUltimaConsulta(), LocalDateTime.now()) > this.INTERVALO_HH_CONSULTAS)
			this.obtenerDatosApi();
		return this.obtenerTemperaturaEnCelsius();
	}
	
	private void obtenerDatosApi() {
		AccuWeatherAPI apiClima = new AccuWeatherAPI();
		condicionesClimaticas = apiClima.getWeather(ciudad.getNombre());
	}
	
	private Double obtenerTemperaturaEnCelsius(){
		HashMap<String, Object> tempObjeto = (HashMap<String, Object>) condicionesClimaticas.get(0).get("Temperature");
		return (((double) tempObjeto.get("Value")) - 32) * 5/9; 
	}
	...
}
```
## Sugerencia de Atuendos
Para la sugerencia de atuendos generé un Asesor que a partir del clima genera sugerencias de Atuendos. Para ello las prendas ahora poseen como atributo el mínimo y máximo de temperatura adecuada de uso.
_Asesor:_
```
public class Asesor {
	List<Prenda> parteInferior;
	List<Prenda> parteSuperior;
	List<Prenda> calzado;
	List<Prenda> accesorios;
	CondicionClimatica condicionClimatica;
	
	...
	
	public Atuendo sugerirAtuendo() {
		return new Atuendo(
			Arrays.asList(this.elegirPrendaSegunTemperatura(parteSuperior)), 
			Arrays.asList(this.elegirPrendaSegunTemperatura(parteInferior)), 
			Arrays.asList(this.elegirPrendaSegunTemperatura(calzado)), 
			Arrays.asList(this.elegirPrendaSegunTemperatura(accesorios))
		);
	}
	
	private Prenda elegirPrendaSegunTemperatura(List<Prenda> prendas) {
		return prendas.stream()
				.filter(p -> p.estaParaUsarA(condicionClimatica.obtenerTemperaturaActual()))
				.collect(Collectors.toList()).get(0);
	}
}
```
## Diagrama de Clases
![Diagrama de clases](https://raw.githubusercontent.com/nicolas-m-gomez/QueMePongo/master/diagrama%20de%20clases%20-%20entrega%204.png)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODU5OTM2MjBdfQ==
-->