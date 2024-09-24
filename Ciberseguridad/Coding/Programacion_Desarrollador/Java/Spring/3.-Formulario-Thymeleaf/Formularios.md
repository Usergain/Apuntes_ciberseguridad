Antes de empezar con los formularios tenemos que revisar que en el *pom* dentro de la dependencia *spring-boot-devtools* este:

![[Pasted image 20240213175459.png]]

Si no apareciese:

![[Pasted image 20240214114503.png]]

![[Pasted image 20240214114317.png]]

![[Pasted image 20240213181824.png]]

*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class FormController {
	@GetMapping("/form")
	public String form(Model model) {
		model.addAttribute("titulo", "Formulario usuarios");
		return "form";
	}

	@PostMapping("/form")
	public String procesar(Model model, 
			@RequestParam(name = "username") String username,
			@RequestParam(name = "password") String password, 
			@RequestParam(name = "email") String email) {
		
		model.addAttribute("titulo", "Resultado form");
		model.addAttribute("username", username);
		model.addAttribute("password", password);
		model.addAttribute("email", email);
		
		return "resultado";
	}
}
```

*form*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<form th:action="@{/form}" method="post">
		<div>
			<label for="username">Username</label>
			<div>
				<input type="text" name="username" id="username">
			</div>
		</div>
		<div>
			<label for="password">Password</label>
			<div>
				<input type="password" name="password" id="password">
			</div>
		</div>
		<div>
			<label for="email">Email</label>
			<div>
				<input type="text" name="email" id="email">
			</div>
		</div>
		<div>
			<div>
				<input type="submit" value="Enviar">
			</div>
		</div>
	</form>
</body>
</html>
```

*resultado*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<ul>
		<li th:text="${username}"></li>
		<li th:text="${password}"></li>
		<li th:text="${email}"></li>
	</ul>
</body>
</html>
```


![[Pasted image 20240213181737.png]]

*-------------------------------------------------------*
*Utilizando Objeto Usuario y mapeando los campos*
*-------------------------------------------------------*
![[Pasted image 20240213183135.png]]

*Usuario*
```java
package com.bolsadeideas.springboot.form.app.models.domain;

public class Usuario {
	private String username;
	private String password;
	private String email;
	
	public String getUsername() {
		return username;
	}
	
	public void setUsername(String username) {
		this.username = username;
	}
	
	public String getPassword() {
		return password;
	}
	
	public void setPassword(String password) {
		this.password = password;
	}
	
	public String getEmail() {
		return email;
	}
	
	public void setEmail(String email) {
		this.email = email;
	}

}
```

*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

@Controller
public class FormController {
	@GetMapping("/form")
	public String form(Model model) {
		model.addAttribute("titulo", "Formulario usuarios");
		return "form";
	}

	@PostMapping("/form")
	public String procesar(Model model, @RequestParam(name = "username") String username,
			@RequestParam(name = "password") String password, @RequestParam(name = "email") String email) {

		Usuario usuario = new Usuario(); //Clase POJO, no es necesario realizar inyección de dependencia
		usuario.setUsername(username);
		usuario.setPassword(password);
		usuario.setEmail(email);

		model.addAttribute("titulo", "Resultado form");
		model.addAttribute("usuario", usuario);

		return "resultado";
	}
}
```

*resultado*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<ul>
		<li th:text="${usuario.username}"></li>
		<li th:text="${usuario.password}"></li>
		<li th:text="${usuario.email}"></li>
	</ul>
</body>
</html>
```

*Poblando la clase Usuario sin @RequestParam*

*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

@Controller
public class FormController {
	@GetMapping("/form")
	public String form(Model model) {
		model.addAttribute("titulo", "Formulario usuarios");
		return "form";
	}

	@PostMapping("/form")
	public String procesar(Usuario usuario, Model model) {
		model.addAttribute("titulo", "Resultado form");
		model.addAttribute("usuario", usuario);
		
		return "resultado";
	}
}
```

*--------------------------------------------*
 *Validacion y mensaje error formulario*
*--------------------------------------------*
*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import java.io.ObjectInputStream.GetField;
import java.util.HashMap;
import java.util.Map;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

import jakarta.validation.Valid;

@Controller
public class FormController {
	@GetMapping("/form")
	public String form(Model model) {
		Usuario usuario = new Usuario();
		model.addAttribute("titulo", "Formulario usuarios");
		model.addAttribute("usuario", usuario);
		return "form";
	}

	@PostMapping("/form")
	public String procesar(@Valid Usuario usuario, BindingResult result, Model model) {

		model.addAttribute("titulo", "Resultado form");

		if (result.hasErrors()) {
			Map<String, String> errores = new HashMap<>();

			result.getFieldErrors().forEach(err -> {
				errores.put(err.getField(),
						"El campo ".concat(err.getField()).concat(" ").concat(err.getDefaultMessage()));
			});
			model.addAttribute("error", errores);
			return "form";
		}
		model.addAttribute("usuario", usuario);
		return "resultado";
	}

}

```

*Usuario* -> con la anotación@NotEmpty
```java
...
public class Usuario {

@NotEmpty
private String username;

@NotEmpty
private String password;

@NotEmpty
private String email
...
```

*form*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<form th:action="@{/form}" method="post">
		<div>
			<label for="username">Username</label>
			<div>
				<input type="text" name="username" id="username" th:value="${usuario.username}">
			</div>
			<div th:if="${error != null && error.containsKey('username')}" th:text="${error.username}">	</div>
		</div>
		<div>
			<label for="password">Password</label>
			<div>
				<input type="password" name="password" id="password">
			</div>
			<div th:if="${error != null && error.containsKey('password')}" th:text="${error.password}"></div>
		</div>
		<div>
			<label for="email">Email</label>
			<div>
				<input type="email" name="email" id="email" th:value="${usuario.email}">
			</div>
			<div th:if="${error != null && error.containsKey('email')}" th:text="${error.email}"></div>
		</div>
		<div>
			<div>
			<input type="submit" value="Enviar">
			</div>
		</div>
	</form>
</body>
</html>
```

*------------------------------*
*Atributo Object thymeleaf*
*------------------------------*
*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.SessionAttributes;
import org.springframework.web.bind.support.SessionStatus;

import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

import jakarta.validation.Valid;

@Controller
@SessionAttributes("usuario")
public class FormController {
	@GetMapping("/form")
	public String form(Model model) {
		Usuario usuario = new Usuario();
		usuario.setNombre("Arkaitz");
		usuario.setApellido("Artaraz");
		model.addAttribute("titulo", "Formulario usuarios");
		model.addAttribute("usuario", usuario);
		return "form";
	}

	@PostMapping("/form")
	public String procesar(@Valid Usuario usuario, BindingResult result, Model model, SessionStatus status) {

		model.addAttribute("titulo", "Resultado form");

		if (result.hasErrors()) {
			return "form";
		}
		model.addAttribute("usuario", usuario);
		return "resultado";
	}

}

```

*Usuario*
```java
package com.bolsadeideas.springboot.form.app.models.domain;

import jakarta.validation.constraints.NotEmpty;
public class Usuario {
	
	@NotEmpty
	private String nombre;
	
	@NotEmpty
	private String apellido;
	
	@NotEmpty
	private String username;
	
	@NotEmpty
	private String password; 
	
	@NotEmpty
	private String email;
	
	public String getNombre() {
		return nombre;
	}
	
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}
	
	public String getApellido() {
		return apellido;
	}
	
	public void setApellido(String apellido) {
		this.apellido = apellido;
	}
	
	public String getUsername() {
		return username;
	} 
	
	public void setUsername(String username) {
		this.username = username;
	}
	
	public String getPassword() {
		return password;
	} 
	
	public void setPassword(String password) {
		this.password = password;
	}
	
	public String getEmail() {
		return email;
	} 
	
	public void setEmail(String email) {
		this.email = email;
	}
}
```

*Resultado*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<ul>
		<li th:text="${usuario.nombre}"></li>
		<li th:text="${usuario.apellido}"></li>
		<li th:text="${usuario.username}"></li>
		<li th:text="${usuario.password}"></li>
		<li th:text="${usuario.email}"></li>
	</ul>
</body>
</html>
```

*form*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<form th:action="@{/form}" th:object="${usuario}" method="post">
		<div>
			<label for="nombre">Nombre</label>
			<div>
				<input type="text" id="nombre" th:field="*{nombre}">
			</div>
			<div th:if="${#fields.hasErrors('nombre')}" th:errors="*{nombre}"></div>
		</div>
		<div>
			<label for="apellido">Apellido</label>
			<div>
				<input type="text" id="apellido" th:field="*{apellido}">
			</div>
			<div th:if="${#fields.hasErrors('apellido')}" th:errors="*{apellido}"></div>
		</div>
		<div>
			<label for="username">Username</label>
			<div>
				<input type="text" id="username" th:field="*{username}">
			</div>
			<div th:if="${#fields.hasErrors('username')}" th:errors="*{username}"></div>
		</div>
		<div>
			<label for="password">Password</label>
			<div>
				<input type="password" th:field="*{password}" id="password">
			</div>
			<div th:if="${#fields.hasErrors('password')}" th:errors="*{password}"></div>
		</div>
		<div>
			<label for="email">Email</label>
			<div>
				<input type="email" id="email" th:field="*{email}">
			</div>
			<div th:if="${#fields.hasErrors('email')}" th:errors="*{email}"></div>
		</div>
		<div>
			<div>
				<input type="submit" value="Enviar">
			</div>
		</div>
	</form>
</body>
</html>
```

*-----------------------------------------------------------*
*@SessionAttributes* para mantener datos durante ciclo
*-----------------------------------------------------------*
Es muy importante para el campo *Id* de una tabla de la BBDD
*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.SessionAttributes;
import org.springframework.web.bind.support.SessionStatus;

import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

import jakarta.validation.Valid;

@Controller
@SessionAttributes("usuario")
public class FormController {
	@GetMapping("/form")
	public String form(Model model) {
		Usuario usuario = new Usuario();
		usuario.setNombre("Arkaitz");
		usuario.setApellido("Artaraz");
		usuario.setIdentificador("123.456.789-K");
		model.addAttribute("titulo", "Formulario usuarios");
		model.addAttribute("usuario", usuario);
		return "form";
	}

	@PostMapping("/form")
	public String procesar(@Valid Usuario usuario, BindingResult result, Model model, SessionStatus status) {

		model.addAttribute("titulo", "Resultado form");

		if (result.hasErrors()) {
			return "form";
		}
		model.addAttribute("usuario", usuario);
		status.setComplete();
		return "resultado";
	}
}

```

*Usuario*
```java
public class Usuario {

	private String identificador;
	...
	...
	public String getIdentificador() {
	
	return identificador;
	}
	
	public void setIdentificador(String identificador) {
	this.identificador = identificador;
	}
```

*resultado
```html
<!DOCTYPE html>
...
...
<li th:text="${usuario.identificador}"></li>
...
...
</body>
</html>
```

*---------------------------*
*Mensajes personalizados*
*---------------------------*
*Una opcion* en *Usuario* colocar el mensaje que se quiere ver o las premisas a rellenar
```java
@NotEmpty(message="El nombre no puede estar vacio")
private String nombre;

@NotEmpty
private String apellido;

@NotEmpty
@Size(min = 3, max = 8)
private String username;

@NotEmpty
private String password;

@NotEmpty
@Email (message="El correo con formato incorrecto")

private String email;
...
...
```

La otra opción es crear archivo *messages.properties*, de esta forma se desacopla.
```properties
NotEmpty=el campo es requerido
NotEmpty.usuario.nombre=el campo nombre es requerido
NotEmpty.usuario.username=el username es obligatorio
Email.usuario.email=el formato del correo es incorrecto, falta el @
```

Configuraremos el archivo *message.properties con UTF-8*
![[Pasted image 20240214170514.png]]

![[Pasted image 20240214170703.png]]
*-----------------------------------------*
*@Pattern para expresiones regulares*
*-----------------------------------------*
*Usuario*
```java
@Pattern(regexp = "[0-9]{2}[.][\\d]{3}[.][\\d]{3}[-][A-Z]{1}")
private String identificador;
...
...
```

*-------------------------------*
*Validaciones personalizadas*
*-------------------------------*
*UsuarioValidator*
```java
package com.bolsadeideas.springboot.form.app.validation;

import org.springframework.stereotype.Component;
import org.springframework.validation.Errors;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.Validator;

import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

@Component
public class UsuarioValidador implements Validator {

	@Override
	public boolean supports(Class<?> clazz) {
		// TODO Auto-generated method stub
		return Usuario.class.isAssignableFrom(clazz);
	}

	@Override
	public void validate(Object target, Errors errors) {
		Usuario usuario = (Usuario) target;

		ValidationUtils.rejectIfEmptyOrWhitespace(errors, "nombre", "requerido.usuario.nombre");

		if (!usuario.getIdentificador().matches("[0-9]{2}[.][\\d]{3}[.][\\d]{3}[-][A-Z]{1}")) {
			errors.rejectValue("identificador", "pattern.usuario.identificador");
		}

	}

}
```

*FormController*
```java
...
@Autowired
private UsuarioValidador validador;

//Evento ciclo de vida del Binder (poblar datos, implementar validaciones, etc)-Un stack de cosas
@InitBinder
public void initBinder(WebDataBinder binder) {

binder.addValidators(validador);
...
...
}
```

*Crear tus propias anotaciones*
![[Pasted image 20240214181424.png]]

![[Pasted image 20240214181636.png]]

![[Pasted image 20240214182321.png]]
Copiar estas líneas de código
![[Pasted image 20240214182354.png]]

![[Pasted image 20240214183002.png]]

*IdentificadorRegexValidator*
```java
package com.bolsadeideas.springboot.form.app.validation;

import jakarta.validation.ConstraintValidator;
import jakarta.validation.ConstraintValidatorContext;

public class IdentificadorRegexValidador implements ConstraintValidator<IdentificadorRegex, String> {

	@Override
	public boolean isValid(String value, ConstraintValidatorContext context) {
		if (value.matches("[0-9]{2}[.][\\d]{3}[.][\\d]{3}[-][A-Z]{1}")) {
		return true;
		}
		return false;
	}

}
```

*IdentificadorRegex*
```java
package com.bolsadeideas.springboot.form.app.validation;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.ElementType.METHOD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;
import jakarta.validation.Constraint;
import jakarta.validation.Payload;

@Constraint(validatedBy = IdentificadorRegexValidador.class)
@Retention(RUNTIME)
@Target({ FIELD, METHOD })
public @interface IdentificadorRegex {
	String message() default "Identificador inválido";

	Class<?>[] groups() default {};

	Class<? extends Payload>[] payload() default {};
}
```

*usuarioValidador*
```java
package com.bolsadeideas.springboot.form.app.validation;

import org.springframework.stereotype.Component;
import org.springframework.validation.Errors;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.Validator;

import com.bolsadeideas.springboot.form.app.models.domain.Usuario;

@Component
public class UsuarioValidador implements Validator {

	@Override
	public boolean supports(Class<?> clazz) {
		// TODO Auto-generated method stub
		return Usuario.class.isAssignableFrom(clazz);
	}

	@Override
	public void validate(Object target, Errors errors) {
	
		ValidationUtils.rejectIfEmptyOrWhitespace(errors, "nombre", "requerido.usuario.nombre");

	}

}
```

*Usuario*
```java
@IdentificadorRegex
private String identificador;
...
...
```

*messages*
```properties
NotEmpty=el campo es requerido
NotEmpty.usuario.nombre=el campo nombre no puede ser vacío
NotEmpty.usuario.username=el username es obligatorio
Email.usuario.email=el formato del correo es inválido, falta el @
pattern.usuario.identificador=formato de la expresión regular incorrecto
requerido.usuario.nombre=el campo nombre es requerido
IdentificadorRegex.usuario.identificador=formato de la expresión regular inválido por anotaciones
Requerido.usuario.apellido=apellido requerido con anotaciones
```


vamos a crear otra anotación para crear una anotación personalizada:
![[Pasted image 20240215100848.png]]

*Requerido*
```java
package com.bolsadeideas.springboot.form.app.validation;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.ElementType.METHOD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import jakarta.validation.Constraint;
import jakarta.validation.Payload;

@Constraint(validatedBy = RequeridoValidador.class)
@Retention(RUNTIME)
@Target({ FIELD, METHOD })
public @interface Requerido {
	String message() default "Campo requerido";

	Class<?>[] groups() default {};

	Class<? extends Payload>[] payload() default {};

}
```

*RequeridoValidador*
```java
package com.bolsadeideas.springboot.form.app.validation;

import org.springframework.util.StringUtils;

import jakarta.validation.ConstraintValidator;
import jakarta.validation.ConstraintValidatorContext;

public class RequeridoValidador implements ConstraintValidator<Requerido, String> {

	@Override
	public boolean isValid(String value, ConstraintValidatorContext context) {
		//if (value == null || value.isEmpty() || value.isBlank()) {
		if (value == null || !StringUtils.hasText(value)) {
			return false;
		}
		return true;
	}
}
```

*Usuario* -> los campos @NotBlank los cambiaremos por @Requerido, anotación personalizada
```java
package com.bolsadeideas.springboot.form.app.models.domain;

import com.bolsadeideas.springboot.form.app.validation.IdentificadorRegex;
import com.bolsadeideas.springboot.form.app.validation.Requerido;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotEmpty;
import jakarta.validation.constraints.Size;

public class Usuario {

	// @Pattern(regexp = "[0-9]{2}[.][\\d]{3}[.][\\d]{3}[-][A-Z]{1}")
	@IdentificadorRegex
	private String identificador;

	// @NotEmpty(message="el nombre no puede ser vacio")
	private String nombre;

	// @NotEmpty
	@Requerido
	private String apellido;

	@NotBlank // Para no permitir espacios en blanco, no los valida
	@Size(min = 3, max = 8)
	private String username;

	@NotEmpty
	private String password;

	//@NotEmpty
	@Requerido
	@Email
	private String email;

```

*-----------------------------------*
 *Validación de números enteros*
*-----------------------------------*
*Usuario*
```java
...
@NotNull
@Min(5)
@Max(5000)
private Integer cuenta;
...
```

*form*
```html
...
<div>
	<label for="cuenta">Cuenta</label>
	<div>
		<input type="number" id="cuenta" th:field="*{cuenta}">
	</div>
	<div th:if="${#fields.hasErrors('cuenta')}" th:errors="*{cuenta}"></div>
</div>
...
```

*resultado*
```html
...
<li th:text="${usuario.cuenta}"></li>
...
```

*messages*
```properties
...
typeMismatch.java.lang.Integer=Debe ser un entero
typeMismatch.usuario.cuenta=El campo Cuenta debe ser un entero
```

*---------------------------------------*
 *Validación y Formateado de fechas*
*---------------------------------------*
*form*
```html
...
<div>
	<label for="fechaNacimiento">Fecha de Nacimiento</label>
	<div>
		<input type="text" id="fechaNacimiento" th:field="*{fechaNacimiento}" placeholder="yyyy/MM/dd">
	</div>
	<div th:if="${#fields.hasErrors('fechaNacimiento')}" th:errors="*{fechaNacimiento}"></div>
</div>
...
```

*Usuario*
```java
...
@NotNull
//@Future

@Past
@DateTimeFormat(pattern="yyyy/MM/dd")
private Date fechaNacimiento;
...
```

*messages*
```properties
...
typeMismatch.java.util.Date=Debe ser una fecha con formato yyyy/MM/dd
typeMismatch.usuario.fechaNacimiento=Debe ser una fecha con formato yyyy/MM/dd
Future.usuario.fechaNacimiento=deb ser una fecha en futuro
...
```

*Init Binder-CustomDateEditor*-> funciona como un filtro para manejar lo que se mete por formulario

*Usuario*->comentamos la etiqueta @DateTimeFormat
```java
@NotNull
//@Future
@Past
//@DateTimeFormat(pattern="yyyy-MM-dd") //el input de html5 lo maneja con guiones-
private Date fechaNacimiento;
```

*FormController*
```java
@InitBinder

public void initBinder(WebDataBinder binder) {
	binder.addValidators(validador);
	SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
	dateFormat.setLenient(false);
	binder.registerCustomEditor(Date.class, "fechaNacimiento", new CustomDateEditor(dateFormat, false));
}
```

*--------------------------------*
 *Implementar propios fliltros*
*--------------------------------*
![[Pasted image 20240226095337.png | 400]]

*NombreMayusculaEditor*
```java
package com.bolsadeideas.springboot.form.app.editors;

import java.beans.PropertyEditorSupport;

public class NombreMayusculaEditor extends PropertyEditorSupport{

	@Override
	public void setAsText(String text) throws IllegalArgumentException {
		setValue(text.toUpperCase().trim());
	}

}
```

*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

....
....

@Controller
@SessionAttributes("usuario")
public class FormController {

	....
	
	// Evento ciclo de vida del Binder (poblar datos, implementar validaciones,
	// etc)-Un stack de cosas
	@InitBinder
	public void initBinder(WebDataBinder binder) {
		binder.addValidators(validador);
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		dateFormat.setLenient(false);
		binder.registerCustomEditor(Date.class, "fechaNacimiento", new CustomDateEditor(dateFormat, false));
		
		//Para customizar filtros, en este caso todo lo rellenado en campo nombre se pasará a mayuscula
		binder.registerCustomEditor(String.class, "nombre", new NombreMayusculaEditor());
	}
	.....
	.....
```

*----------------------------------------------------------------------------------------------------------------------------------------------*
 *----------------------------*
*| Lista select desplegable  |*
 *----------------------------*
*resultado*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
<h3 th:text="${titulo}"></h3>
	<ul>
		<li th:text="${usuario.nombre}"></li>
		<li th:text="${usuario.apellido}"></li>
		<li th:text="${usuario.username}"></li>
		<li th:text="${usuario.password}"></li>
		<li th:text="${usuario.email}"></li>
		<li th:text="${usuario.identificador}"></li>
		<li th:text="${usuario.cuenta}"></li>
		<li th:text="${usuario.pais}"></li>
	</ul>
</body>
</html>
```

*Usuario*
```java
...
...
@NotEmpty
private String pais;
...
...
public String getPais() {
	return pais;
}

public void setPais(String pais) {
	this.pais = pais;
}
```

*FormController*
```java
...
...
@ModelAttribute("paises")
public List<String> paises() {
	return Arrays.asList("España", "Mexico", "Chile", "Argentina", "Perú", "Colombia", "Venezuela");
}
...
...
```

*form*
```html
...
...
	<div>
		<label for="pais">Pais</label>
		<div>
			<select id="pais" th:field="*{pais}">
			<option value="">-- seleccionar --</option>
			<option th:each="pais: ${paises}" th:text="${pais}"
			th:value="${pais}"></option>
			</select>
		</div>
		<div th:if="${#fields.hasErrors('pais')}" th:errors="*{pais}"></div>
	</div>
...
...
```

*Para dar formato a las fechas que aparezcan en pantalla*
*resultado*
```html
...
<li th:text="${#dates.format(usuario.fechaNacimiento, 'dd/MM/yyyy')}"></li>
...
```


*-----------------------*
*Lista select con Map*
*-----------------------*

*FormController*
```java
@ModelAttribute("paisesMap")
public Map<String, String> paisesMap() {
	Map<String, String> paises = new HashMap<String, String>();
	paises.put("ES", "España");
	paises.put("MX", "Mexio");
	paises.put("CL", "Chile");
	paises.put("AR", "Argentina");
	paises.put("PE", "Perú");
	paises.put("CO", "Colombia");
	paises.put("VE", "Venezuela");
	return paises;
}
```

*form*
```html
<div>
	<label for="pais">Pais</label>
	<div>
		<select id="pais" th:field="*{pais}">
			<option value="">-- seleccionar --</option>
			<option th:each="pais: ${paisesMap.entrySet()}" th:text="${pais.value}"
			th:value="${pais.key}"></option>
		</select>
	</div>
	<div th:if="${#fields.hasErrors('pais')}" th:errors="*{pais}"></div>
</div>
```

![[Pasted image 20240226115936.png]]

*----------------------------------*
*Lista select con Map de Objetos*
*----------------------------------*
*FormController*
```java
@ModelAttribute("listaPaises")
public List<Pais> listaPaises() {
	return Arrays.asList(
		new Pais(1, "ES", "España"),
		new Pais(2, "MX", "Mexico"),
		new Pais(3, "CL", "Chile"),
		new Pais(4, "AR", "Argentina"),
		new Pais(5, "PE", "Perú"),
		new Pais(6, "CO", "Colombia"),
		new Pais(7, "VE", "Venezuela"));
	}
```

*form*
```html
<div>
	<label for="pais.codigo">Pais</label>
	<div>
		<select id="pais" th:field="*{pais.codigo}">
			<option value="">-- seleccionar --</option>
			<option th:each="pais: ${listaPaises}" th:text="${pais.nombre}"
			th:value="${pais.codigo}"></option>
		</select>
	</div>
	<div th:if="${#fields.hasErrors('pais.codigo')}" th:errors="*{pais.codigo}"></div>
</div>
```

*Usuario*
```java
...
@Valid
private Pais pais;
...
```

*Pais*
```java
public class Pais {

	private Integer id;
	@NotEmpty
	private String codigo;
	private String nombre;
...
```

*resultado*
```html
<li th:text="${usuario.pais.codigo}"></li>
```

*---------------------------------------------------------*
*Componente Service de Objeto con PropertyEditors*
*---------------------------------------------------------*
De esta manera creamos una clase que se encarga de operar con los datos, asi reutilizarla las veces que queramos. Estaremos trayendo el objeto completo, no atributos de la clase. Convertirmos el id en el Objeto completo utilizando PropertyEditors.

![[Pasted image 20240226181406.png |350]]

*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Arrays;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.propertyeditors.CustomDateEditor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.SessionAttributes;
import org.springframework.web.bind.support.SessionStatus;

import com.bolsadeideas.springboot.form.app.editors.NombreMayusculaEditor;
import com.bolsadeideas.springboot.form.app.editors.PaisPropertyEditor;
import com.bolsadeideas.springboot.form.app.models.domain.Pais;
import com.bolsadeideas.springboot.form.app.models.domain.Usuario;
import com.bolsadeideas.springboot.form.app.services.PaisService;
import com.bolsadeideas.springboot.form.app.validation.UsuarioValidador;

import jakarta.validation.Valid;

@Controller
@SessionAttributes("usuario")
public class FormController {

	@Autowired
	private UsuarioValidador validador;
	
	@Autowired
	private PaisService paisService;
	
	@Autowired
private PaisPropertyEditor paisEditor;

	// Evento ciclo de vida del Binder (poblar datos, implementar validaciones,
	// etc)-Un stack de cosas
	@InitBinder
	public void initBinder(WebDataBinder binder) {
		binder.addValidators(validador);
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		dateFormat.setLenient(false);
		binder.registerCustomEditor(Date.class, "fechaNacimiento", new CustomDateEditor(dateFormat, false));

		// Para customizar filtros, en este caso todo lo rellenado en campo nombre se
		// pasará a mayuscula
		binder.registerCustomEditor(String.class, "nombre", new NombreMayusculaEditor());
		
		binder.registerCustomEditor(Pais.class, "pais", paisEditor);
	}

	@ModelAttribute("listaPaises")
	public List<Pais> listaPaises() {
		return paisService.listar();
	}

	@ModelAttribute("paises")
	public List<String> paises() {
		return Arrays.asList("España", "Mexico", "Chile", "Argentina", "Perú", "Colombia", "Venezuela");
	}

	@ModelAttribute("paisesMap")
	public Map<String, String> paisesMap() {
		Map<String, String> paises = new HashMap<String, String>();
		paises.put("ES", "España");
		paises.put("MX", "Mexio");
		paises.put("CL", "Chile");
		paises.put("AR", "Argentina");
		paises.put("PE", "Perú");
		paises.put("CO", "Colombia");
		paises.put("VE", "Venezuela");
		return paises;
	}

	@GetMapping("/form")
	public String form(Model model) {
		Usuario usuario = new Usuario();
		usuario.setNombre("Arkaitz");
		usuario.setApellido("Artaraz");
		usuario.setIdentificador("12.456.789-K");
		model.addAttribute("titulo", "Formulario usuarios");
		model.addAttribute("usuario", usuario);
		return "form";
	}

	@PostMapping("/form")
	public String procesar(@Valid Usuario usuario, BindingResult result, Model model, SessionStatus status) {

		// validador.validate(usuario, result);

		model.addAttribute("titulo", "Resultado form");

		if (result.hasErrors()) {
			/*
			 * Map<String, String> errores = new HashMap<>();
			 * 
			 * result.getFieldErrors().forEach(err -> { errores.put(err.getField(),
			 * "El campo ".concat(err.getField()).concat(" ").concat(err.getDefaultMessage()
			 * )); }); model.addAttribute("error", errores);
			 */
			return "form";

			/*
			 * Usuario usuario = new Usuario(); // Clase POJO, no es necesario realizar
			 * inyección de dependencia usuario.setUsername(username);
			 * usuario.setPassword(password); usuario.setEmail(email);
			 */

			/*
			 * model.addAttribute("username", username); model.addAttribute("password",
			 * password); model.addAttribute("email", email);
			 */

		}
		model.addAttribute("usuario", usuario);
		status.setComplete();
		return "resultado";
	}

}

```

*PaisService*
```java
package com.bolsadeideas.springboot.form.app.services;

import java.util.List;
import com.bolsadeideas.springboot.form.app.models.domain.Pais;

public interface PaisService {
	public List<Pais> listar();
	public Pais obtenerPorId(Integer id);
}
```

*PaisServiceImpl*
```java
package com.bolsadeideas.springboot.form.app.services;

import java.util.Arrays;
import java.util.List;

import org.springframework.stereotype.Service;

import com.bolsadeideas.springboot.form.app.models.domain.Pais;

@Service
public class PaisServiceImpl implements PaisService{
	
	private List<Pais> lista;

	public PaisServiceImpl() {
		this.lista = Arrays.asList(
				new Pais(1, "ES", "España"), 
				new Pais(2, "MX", "Mexico"), 
				new Pais(3, "CL", "Chile"),
				new Pais(4, "AR", "Argentina"), 
				new Pais(5, "PE", "Perú"), 
				new Pais(6, "CO", "Colombia"),
				new Pais(7, "VE", "Venezuela"));
	}

	@Override
	public List<Pais> listar() {
		return lista;
	}

	@Override
	public Pais obtenerPorId(Integer id) {
		Pais resultado = null;
		for(Pais pais: this.lista) {
			if(id==pais.getId()) {
				resultado=pais;
				break;
			}
		}
		return resultado;
	}

}
```

*PaisPropertyEditor*
```java
package com.bolsadeideas.springboot.form.app.editors;

import java.beans.PropertyEditorSupport;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import com.bolsadeideas.springboot.form.app.services.PaisService;

@Component
public class PaisPropertyEditor extends PropertyEditorSupport {

	@Autowired
	private PaisService service;

	@Override
	public void setAsText(String idString) throws IllegalArgumentException {
			try {
				Integer id = Integer.parseInt(idString);
				this.setValue(service.obtenerPorId(id));
			} catch (NumberFormatException e) {
				setValue(null);
			}
		}
	}
```

*Pais*
```java
package com.bolsadeideas.springboot.form.app.models.domain;

import jakarta.validation.constraints.NotEmpty;
import jakarta.validation.constraints.NotNull;

public class Pais {
	
	private Integer id;
	private String codigo;
	private String nombre;

	public Pais() {

	}

	public Pais(Integer id, String codigo, String nombre) {
		this.id = id;
		this.codigo = codigo;
		this.nombre = nombre;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getCodigo() {
		return codigo;
	}

	public void setCodigo(String codigo) {
		this.codigo = codigo;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

}

```

*Usuario*
```java
package com.bolsadeideas.springboot.form.app.models.domain;

import java.util.Date;


import com.bolsadeideas.springboot.form.app.validation.IdentificadorRegex;
import com.bolsadeideas.springboot.form.app.validation.Requerido;

import jakarta.validation.Valid;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.Max;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotEmpty;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Past;
import jakarta.validation.constraints.Size;

public class Usuario {

	@IdentificadorRegex
	private String identificador;

	private String nombre;

	@Requerido
	private String apellido;

	@NotBlank //Para no permitir espacios en blanco, no los valida
	@Size(min = 3, max = 8)
	private String username;

	@NotEmpty
	private String password;

	@Requerido
	@Email
	private String email;

	@NotNull
	@Min(5)
	@Max(5000)
	private Integer cuenta;
	
	@NotNull
	@Past
	private Date fechaNacimiento;
	
	@NotNull
	private Pais pais;
	

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public String getApellido() {
		return apellido;
	}

	public void setApellido(String apellido) {
		this.apellido = apellido;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getIdentificador() {
		return identificador;
	}

	public void setIdentificador(String identificador) {
		this.identificador = identificador;
	}

	public Integer getCuenta() {
		return cuenta;
	}

	public void setCuenta(Integer cuenta) {
		this.cuenta = cuenta;
	}

	public Date getFechaNacimiento() {
		return fechaNacimiento;
	}

	public void setFechaNacimiento(Date fechaNacimiento) {
		this.fechaNacimiento = fechaNacimiento;
	}

	public Pais getPais() {
		return pais;
	}

	public void setPais(Pais pais) {
		this.pais = pais;
	}
	
}

```

*resultado*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<ul>
		<li th:text="${usuario.nombre}"></li>
		<li th:text="${usuario.apellido}"></li>
		<li th:text="${usuario.username}"></li>
		<li th:text="${usuario.password}"></li>
		<li th:text="${usuario.email}"></li>
		<li th:text="${usuario.identificador}"></li>
		<li th:text="${usuario.cuenta}"></li>
		<li th:text="${usuario.pais.id}"></li>
		<li th:text="${usuario.pais.codigo}"></li>
		<li th:text="${usuario.pais.nombre}"></li>
		<li th:text="${#dates.format(usuario.fechaNacimiento, 'dd/MM/yyyy')}"></li>
	</ul>
</body>
</html>
```

*form*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<form th:action="@{/form}" th:object="${usuario}" method="post">
	
		<div>
			<label for="pais">Pais</label>
			<div>
				<select id="pais" th:field="*{pais}">
					<option value="">-- seleccionar --</option>
					<option th:each="pais: ${listaPaises}" th:text="${pais.nombre}"
					th:value="${pais.id}"></option>
				</select>
			</div>
			<div th:if="${#fields.hasErrors('pais')}" th:errors="*{pais}"></div>
		</div>
		
		<div>
			<label for="identificador">Identificador</label>
			<div>
				<input type="text" id="identificador" th:field="*{identificador}">
			</div>
			<div th:if="${#fields.hasErrors('identificador')}"
				th:errors="*{identificador}"></div>
			</div>
		<div>
			<label for="nombre">Nombre</label>
			<div>
				<input type="text" id="nombre" th:field="*{nombre}">
			</div>
			<div th:if="${#fields.hasErrors('nombre')}" th:errors="*{nombre}"></div>
		</div>
		<div>
			<label for="apellido">Apellido</label>
			<div>
				<input type="text" id="apellido" th:field="*{apellido}">
			</div>
			<div th:if="${#fields.hasErrors('apellido')}" th:errors="*{apellido}"></div>
		</div>
		<div>
			<label for="username">Username</label>
			<div>
				<input type="text" id="username" th:field="*{username}">
			</div>
			<div th:if="${#fields.hasErrors('username')}" th:errors="*{username}"></div>
		</div>
		<div>
			<label for="password">Password</label>
			<div>
				<input type="password" th:field="*{password}" id="password">
			</div>
			<div th:if="${#fields.hasErrors('password')}" th:errors="*{password}"></div>
		</div>
		<div>
			<label for="email">Email</label>
			<div>
				<input type="email" id="email" th:field="*{email}">
			</div>
			<div th:if="${#fields.hasErrors('email')}" th:errors="*{email}"></div>
		</div>
		<div>
			<label for="cuenta">Cuenta</label>
			<div>
				<input type="number" id="cuenta" th:field="*{cuenta}">
			</div>
			<div th:if="${#fields.hasErrors('cuenta')}" th:errors="*{cuenta}"></div>
		</div>
		<div>
			<label for="fechaNacimiento">Fecha Nacimiento</label>
			<div>
				<input type="date" id="fechaNacimiento"
				th:field="*{fechaNacimiento}" placeholder="yyyy/MM/dd">
			</div>
			<div th:if="${#fields.hasErrors('fechaNacimiento')}"
				th:errors="*{fechaNacimiento}"></div>
		</div>
		<div>
			<div>
				<input type="submit" value="Enviar">
			</div>
		</div>
	</form>
</body>
</html>
```

*----------------------------------------------------------------------------------------------------------------------------------------------*
*-----------*
*CheckBox*
*-----------*
*CheckBox con Objetos de tipo Role - Property Editor* 

![[Pasted image 20240226203433.png | 400]]

*Usuario*
```java
@NotEmpty
private List<Role> roles;

public List<Role> getRoles() {
	return roles;
}

public void setRoles(List<Role> roles) {
	this.roles = roles;
}
```

*Role*
```java
package com.bolsadeideas.springboot.form.app.models.domain;

public class Role {

	private Integer id;
	private String nombre;
	private String role;
	
	public Role() {
}

	public Role(Integer id, String nombre, String role) {
		super();
		this.id = id;
		this.nombre = nombre;
		this.role = role;
	}
	
	public Integer getId() {
		return id;
	}
	
	public void setId(Integer id) {
		this.id = id;
	}
	
	public String getNombre() {
		return nombre;
	}
	
	public void setNombre(String nombre) {
		this.nombre = nombre;
	}
	
	public String getRole() {
		return role;
	}
	
	public void setRole(String role) {
		this.role = role;
	}
}
```

*RolesEditor*
```java
package com.bolsadeideas.springboot.form.app.editors;
import java.beans.PropertyEditorSupport;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import com.bolsadeideas.springboot.form.app.services.RoleService;

@Component
public class RolesEditor extends PropertyEditorSupport {

	@Autowired
	private RoleService service;

	@Override
	public void setAsText(String text) throws IllegalArgumentException {
		try {
			Integer id = Integer.parseInt(text);
			setValue(service.obtenerPorId(id));
		} catch (NumberFormatException e) {
			setValue(null);
		}
	}
}
```

*RoleService*
```java
package com.bolsadeideas.springboot.form.app.services;

import java.util.List;
import com.bolsadeideas.springboot.form.app.models.domain.Role;

public interface RoleService {

	public List<Role> listar();

	public Role obtenerPorId(Integer id);
}
```

*RoleServiceImpl*
```java
package com.bolsadeideas.springboot.form.app.services;

import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Service;

import com.bolsadeideas.springboot.form.app.models.domain.Role;

@Service
public class RoleServiceImpl implements RoleService {

	private List<Role> roles;

	public RoleServiceImpl() {
		this.roles = new ArrayList<>();
		this.roles.add(new Role(1, "Administrador", "ROLE_ADMIN"));
		this.roles.add(new Role(2, "Usuario", "ROLE_USER"));
		this.roles.add(new Role(3, "Moderador", "ROLE_MODERATOR"));
	}

	@Override
	public List<Role> listar() {
		return roles;
	}

	@Override
	public Role obtenerPorId(Integer id) {
		Role resultado = null;
		for (Role role : roles) {
			if (id == role.getId()) {
				resultado = role;
				break;
			}
		}
		return resultado;
	}
}
```

*FormController*
```java
package com.bolsadeideas.springboot.form.app.controllers;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.propertyeditors.CustomDateEditor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.SessionAttributes;
import org.springframework.web.bind.support.SessionStatus;

import com.bolsadeideas.springboot.form.app.editors.NombreMayusculaEditor;
import com.bolsadeideas.springboot.form.app.editors.PaisPropertyEditor;
import com.bolsadeideas.springboot.form.app.editors.RolesEditor;
import com.bolsadeideas.springboot.form.app.models.domain.Pais;
import com.bolsadeideas.springboot.form.app.models.domain.Role;
import com.bolsadeideas.springboot.form.app.models.domain.Usuario;
import com.bolsadeideas.springboot.form.app.services.PaisService;
import com.bolsadeideas.springboot.form.app.services.RoleService;
import com.bolsadeideas.springboot.form.app.validation.UsuarioValidador;

import jakarta.validation.Valid;

@Controller
@SessionAttributes("usuario")
public class FormController {

	@Autowired
	private UsuarioValidador validador;

	@Autowired
	private PaisService paisService;

	@Autowired
	private RoleService roleService;

	@Autowired
	private PaisPropertyEditor paisEditor;
	
	@Autowired
	private RolesEditor roleEditor;

	// Evento ciclo de vida del Binder (poblar datos, implementar validaciones,
	// etc)-Un stack de cosas
	@InitBinder
	public void initBinder(WebDataBinder binder) {
		binder.addValidators(validador);
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		dateFormat.setLenient(false);
		binder.registerCustomEditor(Date.class, "fechaNacimiento", new CustomDateEditor(dateFormat, false));

		// Para customizar filtros, en este caso todo lo rellenado en campo nombre se
		// pasará a mayuscula
		binder.registerCustomEditor(String.class, "nombre", new NombreMayusculaEditor());

		binder.registerCustomEditor(Pais.class, "pais", paisEditor);
		binder.registerCustomEditor(Role.class, "roles", roleEditor);
	}
	...
	...

	@ModelAttribute("listaRoles")
	public List<Role> listaRoles(){
		return this.roleService.listar();
	}

/*Opcion cuando no teniamos la clase Role, RoleService, RoleServiceImpl, RolesEditor 
	@ModelAttribute("listaRolesString")
	public List<String> listaRolesString() {
	List<String> roles = new ArrayList<>();
		roles.add("ROLE_ADMIN");
		roles.add("ROLE_USER");
		roles.add("ROLE_MODERATOR");
		return roles;
	}
	
	@ModelAttribute("listaRolesMap")
	public Map<String, String> listaRolesMap() {
	Map<String, String> roles = new HashMap<String, String>();
		roles.put("ROLE_ADMIN", "Administrador");
		roles.put("ROLE_USER", "Usuario");
		roles.put("ROLE_MODERATOR", "Moderador");
		return roles;
	}
	*/
```

*form*
```html
...
...
<div>
	<label>Roles</label>
	<div th:each="role: ${listaRoles}">
		<input type="checkbox" th:field="*{roles}" th:value="${role.id}">
		<label th:for="${#ids.prev('roles')}" th:text="${role.nombre}"></label>
	</div>
	<div th:if="${#fields.hasErrors('roles')}"
	th:errors="*{roles}"></div>
</div>
...
...
```

![[Pasted image 20240226202951.png]]

![[Pasted image 20240226202927.png]]

*Checkbox booleano true o false*

![[Pasted image 20240226211934.png]]
![[Pasted image 20240226211902.png]]

*Usuario*
```java
...
private Boolean habilitar;
...
```

*FormController*
```java
@GetMapping("/form")
public String form(Model model) {
	Usuario usuario = new Usuario();
	usuario.setNombre("Arkaitz");
	usuario.setApellido("Artaraz");
	usuario.setIdentificador("12.456.789-K");
	usuario.setHabilitar(true);
	model.addAttribute("titulo", "Formulario usuarios");
	model.addAttribute("usuario", usuario);
	return "form";
}
```

*resultado*
```html
...
<li th:text="'Habilitado: ' + ${usuario.habilitar}"></li>
...
```

*form*
```html
...
<div>
	<label for="habilitar">Habilitar</label>
	<div>
		<input type="checkbox" id="habilitar" th:field="*{habilitar}">
	</div>
</div>
...
```

*------------------------------------------------------------------------------------------------------------------------------------------*
*Rasio Button*

De forma estatica:
*form*
```html
<div>
	<label>Género</label>
	<div>
		<input type="radio" id="genero1" th:field="*{genero}" value="Hombre">
		<label for="genero1">Hombre</label>
	</div>
	<div>
		<input type="radio" id="genero2" th:field="*{genero}" value="Mujer">
		<label for="genero2">Mujer</label>
	</div>
	<div th:if="${#fields.hasErrors('genero')}"
	th:errors="genero"></div>
</div>
```

*Usuario*
```java
@NotEmpty
private String genero;
```

*resultado*
```html
<li th:text="'Genero ' + ${usuario.genero}"></li>
```

De forma dinámica:

*FormController*
```java
...
@ModelAttribute("genero")
public List<String> genero() {
	return Arrays.asList("Hombre", "Mujer");
}
...
```

*form*
```html
...
<div>
	<label>Género</label>
	<div th:each="gen: ${genero}">
		<input type="radio" th:field="*{genero}" th:value="${gen}">
		<label th:for="${#ids.prev('genero')}" th:text="${gen}"></label>
	</div>
	<div th:if="${#fields.hasErrors('genero')}"
	th:errors="genero"></div>
</div>
...
```

*----------------*
 *Input Hidden*
*----------------*

*Usuario*
```java
...
@NotEmpty
private String valorSecreto;
...
```

*FormController*
```java
...
@GetMapping("/form")
public String form(Model model) {
	Usuario usuario = new Usuario();
	usuario.setNombre("Arkaitz");
	usuario.setApellido("Artaraz");
	usuario.setIdentificador("12.456.789-K");
	usuario.setHabilitar(true);
	usuario.setValorSecreto("Algún valor secreto ****");
	model.addAttribute("titulo", "Formulario usuarios");
	model.addAttribute("usuario", usuario);
	return "form";
...
}
```

*form*
```java
...
<div>
	<input type="submit" value="Enviar">
</div>
...
```

*resultado*
```html
<li th:text="'Valor oculto ' + ${usuario.valorSecreto}"></li>
```

*---------------------------*
*Poblando países y roles*
*---------------------------*
*FormController*
```java
@GetMapping("/form")
public String form(Model model) {
	...
	...
	usuario.setPais(new Pais(3, "CL", "Chile"));
	usuario.setRoles(Arrays.asList(new Role(2, "Usuario", "ROLE_USER")));
	return "form";
}
```

*Pais*
```java...
@Override
public String toString() {
	return this.id.toString();
}
```

*Role*
```java
...
...
@Override
public boolean equals(Object obj) {
	if (this == obj) {
		return true;
	}
	if (!(obj instanceof Role)) {
		return false;
	}
	Role role = (Role) obj;
	return this.id != null && this.id.equals(role.getId());
}
...
...
```

*form*
```html
...
<div>
	<label for="pais">Pais</label>
	<div>
		<select id="pais" th:field="*{pais}">
			<option value="">-- seleccionar --</option>
			<option th:each="pais: ${listaPaises}" th:text="${pais.nombre}"
			th:value="${pais.id}"></option>
		</select>
	</div>
	<div th:if="${#fields.hasErrors('pais')}" th:errors="*{pais}"></div>
</div>
<div>
	<label>Roles</label>
	<div th:each="role: ${listaRoles}">
		<input type="checkbox" th:field="*{roles}" th:value="${role.id}"
		th:checked="${#lists.contains(usuario.roles, role)}"> <label
		th:for="${#ids.prev('roles')}" th:text="${role.nombre}"></label>
	</div>
	<div th:if="${#fields.hasErrors('roles')}" th:errors="*{roles}"></div>
</div>
...
```

![[Pasted image 20240227174327.png]]

*Redirect después del POST*
Al actualizar o refrescar la pantalla se vuelve a enviar los datos, para arreglar esto -> REDIRECT a otra página:

*FormController*
```java
...
...
@PostMapping("/form")
public String procesar(@Valid Usuario usuario, BindingResult result, Model model) {

	if (result.hasErrors()) {
	model.addAttribute("titulo", "Resultado form");
	return "form";
	}
	return "redirect:/ver";
}

@GetMapping("/ver")
public String ver(@SessionAttribute(name = "usuario", required = false) Usuario usuario, Model model, SessionStatus status) {
	if (usuario == null) {
	return "redirect:/form";
	}
	model.addAttribute("titulo", "Resultado form");
	
	status.setComplete();
	return "resultado";
}
...
...
```

![[Pasted image 20240227175425.png]]

*--------------------*
  *Estilo Bootstrap*
*--------------------*
https://getbootstrap.com/docs/5.3/getting-started/introduction/

*De forma remota, por la nube*
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
```

![[Pasted image 20240228114252.png | 300]]

*De forma local* entrando por navegador a la href de arriba, copiamos el contenido que aparece y lo guardamos en la carpeta estatico que hemos creado anteriormente.
![[Pasted image 20240228113939.png]]

*form*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
<link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
</head>
<body>
	<h3 th:text="${titulo}"></h3>
	<div class="container">
		<form th:action="@{/form}" th:object="${usuario}" method="post">
		
			<div class="form-group row">
				<label for="habilitar" class="col-form-label col-sm-2">Habilitar</label>
				<div class="row col-sm-4">
					<input type="checkbox" id="habilitar" th:field="*{habilitar}"
					class="form-control col-sm-1">
				</div>
			</div>
			
			<div class="form-group row">
				<label class="col-form-label col-sm-2">Género</label>
				<div th:each="gen: ${genero}" class="row col-sm-2">
					<input type="radio" th:field="*{genero}" th:value="${gen}"
					class="form-control col-sm-2"> <label
					th:for="${#ids.prev('genero')}" th:text="${gen}" class="col-form-label col-sm-2"></label>
				</div>
				<div th:if="${#fields.hasErrors('genero')}" th:errors="*{genero}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="pais" class="col-form-label col-sm-2">Pais</label>
				<div class="col-sm-4">
					<select id="pais" th:field="*{pais}" class="form-control">
						<option value="">-- seleccionar --</option>
						<option th:each="pais: ${listaPaises}" th:text="${pais.nombre}"
						th:value="${pais.id}"></option>
					</select>
				</div>
				<div th:if="${#fields.hasErrors('pais')}" th:errors="*{pais}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label class="col-form-label col-sm-2">Roles</label>
				<div th:each="role: ${listaRoles}" class="row col-sm-2">
					<input type="checkbox" th:field="*{roles}" th:value="${role.id}"
					th:checked="${#lists.contains(usuario.roles, role)}" class="form-control col-sm-2"> 
					<label th:for="${#ids.prev('roles')}" th:text="${role.nombre}" class="col-form-label col-sm-2"></label>
				</div>
				<div th:if="${#fields.hasErrors('roles')}" th:errors="*{roles}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="identificador" class="col-form-label col-sm-2">Identificador</label>
				<div class="col-sm-4">
					<input type="text" id="identificador" th:field="*{identificador}" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('identificador')}"
				th:errors="*{identificador}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="nombre" class="col-form-label col-sm-2">Nombre</label>
				<div class="col-sm-4">
					<input type="text" id="nombre" th:field="*{nombre}" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('nombre')}" th:errors="*{nombre}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="apellido" class="col-form-label col-sm-2">Apellido</label>
				<div class="col-sm-4">
					<input type="text" id="apellido" th:field="*{apellido}" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('apellido')}" th:errors="*{apellido}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="username" class="col-form-label col-sm-2">Username</label>
				<div class="col-sm-4">
					<input type="text" id="username" th:field="*{username}" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('username')}" th:errors="*{username}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="password" class="col-form-label col-sm-2">Password</label>
				<div class="col-sm-4">
					<input type="password" th:field="*{password}" id="password" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('password')}" th:errors="*{password}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="email" class="col-form-label col-sm-2">Email</label>
				<div class="col-sm-4">
					<input type="email" id="email" th:field="*{email}" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('email')}" th:errors="*{email}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="cuenta" class="col-form-label col-sm-2">Cuenta</label>
				<div class="col-sm-4">
					<input type="number" id="cuenta" th:field="*{cuenta}" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('cuenta')}" th:errors="*{cuenta}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<label for="fechaNacimiento" class="col-form-label col-sm-2">Fecha Nacimiento</label>
				<div class="col-sm-4">
					<input type="date" id="fechaNacimiento" th:field="*{fechaNacimiento}" placeholder="yyyy/MM/dd" class="form-control">
				</div>
				<div th:if="${#fields.hasErrors('fechaNacimiento')}" th:errors="*{fechaNacimiento}" class="alert alert-danger"></div>
			</div>
			
			<div class="form-group row">
				<div>
					<input type="submit" value="Enviar" class="btn btn-primary">
				</div>
			</div>
			<input type="hidden" th:field="*{valorSecreto}">
			
		</form>
	</div>
</body>
</html>
```

*resultado*
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title th:text="${titulo}"></title>
<link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
</head>
<body>
	<div class="container">
		<a th:href="@{/form}" class="btn btn-success btn-lg my-3">volver</a>
			<ul class="list-group my-2">
			<li class="list-group-item active" th:text="${titulo}"></li>
			<li class="list-group-item" th:text="${usuario.nombre}"></li>
			<li class="list-group-item" th:text="${usuario.apellido}"></li>
			<li class="list-group-item" th:text="${usuario.username}"></li>
			<li class="list-group-item" th:text="${usuario.password}"></li>
			<li class="list-group-item" th:text="${usuario.email}"></li>
			<li class="list-group-item" th:text="${usuario.identificador}"></li>
			<li class="list-group-item" th:text="${usuario.cuenta}"></li>
			<li class="list-group-item" th:text="${usuario.pais.id}"></li>
			<li class="list-group-item" th:text="${usuario.pais.codigo}"></li>
			<li class="list-group-item" th:text="${usuario.pais.nombre}"></li>
			<li class="list-group-item"
			th:text="${#dates.format(usuario.fechaNacimiento, 'dd/MM/yyyy')}"></li>
			<li class="list-group-item"
			th:text="'Habilitado: ' + ${usuario.habilitar}"></li>
			<li class="list-group-item" th:text="'Genero ' + ${usuario.genero}"></li>
			<li class="list-group-item"
			th:text="'Valor oculto ' + ${usuario.valorSecreto}"></li>
		</ul>
		
		<ul class="list-group my-3">
			<li class="list-group-item active" th:text="'Roles'"></li>
			<li th:each="role: ${usuario.roles}"
			th:text="${role.id} + ': ' + ${role.nombre} + ' -> ' + ${role.role}"></li>
		</ul>
	</div>
</body>
</html>
```


