# Blog For Students Of Computer Science
## Características:
### Registro de usuario / inicio de sesión / cierre de sesión

Los usuarios pueden registrarse y posteriormente iniciar sesión y cerrar sesión. Regístrese con un nombre de usuario, correo electrónico y contraseña. No importa si el correo electrónico es real o falso, siempre que tenga un formato de correo electrónico típico. La contraseña está codificada y salada.

### Publicaciones de blog

Actualmente, solo el usuario con estado de administrador (yo) puede crear, editar y eliminar publicaciones de blog. Las publicaciones se han paginado a 5 publicaciones por página para no atascar el sitio.

### Comentarios

Todos los usuarios registrados pueden publicar comentarios en las publicaciones del blog. Los comentarios pueden ser eliminados por el usuario que los creó y el usuario administrador.

## Laboratorio Nro 09 - Estilos Utilizados
### Codegolf
Codegolf es un tipo de competencia recreativa de programación de computadoras en la que los participantes se esfuerzan por lograr el código fuente más corto posible que resuelva un problema determinado.
Por ejemplo: Utilizamos el método validate_on_submit en vez de crear más código implementando la validación de acuerdo al método que se ha utilizado, ya sea GET o POST.

```python
@app.route('/login', methods=["GET", "POST"])
def login():
    form = LoginForm()
    if form.validate_on_submit():
        email = form.email.data
        password = form.password.data

        user = User.query.filter_by(email=email).first()
        # Email doesn't exist or password incorrect.
        if not user:
            flash("That email does not exist, please try again.")
            return redirect(url_for('login'))
        elif not check_password_hash(user.password, password):
            flash('Password incorrect, please try again.')
            return redirect(url_for('login'))
        else:
            login_user(user)
            return redirect(url_for('get_all_posts'))
    return render_template("login.html", form=form, current_user=current_user)
```

### Cookbook
Un Cookbook en el contexto de la programación es una colección de pequeños programas que demuestran un concepto de programación particular. El método Cookbook es el proceso de aprender un lenguaje de programación mediante la creación de un repositorio de pequeños programas que implementan conceptos de programación específicos.
Por ejemplo, utilizamos este estilo en la creación de formularios por el programador futuro, para que puedan ser reutilizados con mayor facilidad y practicidad.

#### Creación de un formulario con método post
```python
class CreatePostForm(FlaskForm):
    title = StringField("Blog Post Title", validators=[DataRequired()])
    subtitle = StringField("Subtitle", validators=[DataRequired()])
    img_url = StringField("Blog Image URL", validators=[DataRequired(), URL()])
    body = CKEditorField("Blog Content", validators=[DataRequired()])
    submit = SubmitField("Submit Post")
```
#### Creación de un formulario de registro con método post
```python
class RegisterForm(FlaskForm):
    email = StringField("Email", validators=[DataRequired()])
    password = PasswordField("Password", validators=[DataRequired()])
    name = StringField("Name", validators=[DataRequired()])
    submit = SubmitField("Sign Me Up!")
```

#### Creación de logeo de un usuario con método post
```python
class LoginForm(FlaskForm):
    email = StringField("Email", validators=[DataRequired()])
    password = PasswordField("Password", validators=[DataRequired()])
    submit = SubmitField("Let Me In!")
```

#### Creación de formulario de comentario de usuario con método post
```python
class CommentForm(FlaskForm):
    comment_text = CKEditorField("Comment", validators=[DataRequired()])
    submit = SubmitField("Submit Comment")
```

https://user-images.githubusercontent.com/78099176/182956245-cc774297-f4fd-4ca9-a19c-96ce24abd397.mp4

