https://user-images.githubusercontent.com/78099176/182956245-cc774297-f4fd-4ca9-a19c-96ce24abd397.mp4

# Blog For Students Of Computer Science
## ⚔️ Propósito del proyecto ⚔️
El propósito del presente proyecto es realizar un software que ayude a los estudiantes de las Ciencias de la computación a estar informados acerca de los últimos eventos que se están desarrollando a través de la semana de la Computación. Este software ayudará positivamente al desarrollo progresivo de la semana de la Computación y también mostrará información necesaria para el estudiante de la Universidad, Que desee acercarse a uno de sus temas de interés.

## ⚔️ Funcionalidades ⚔️
### Registro de usuario / inicio de sesión / cierre de sesión

Los usuarios pueden registrarse y posteriormente iniciar sesión y cerrar sesión. Regístrese con un nombre de usuario, correo electrónico y contraseña. No importa si el correo electrónico es real o falso, siempre que tenga un formato de correo electrónico típico. La contraseña está codificada y salada.

### Publicaciones de blog

Actualmente, solo el usuario con estado de administrador (yo) puede crear, editar y eliminar publicaciones de blog. Las publicaciones se han paginado a 5 publicaciones por página para no atascar el sitio.

### Comentarios

Todos los usuarios registrados pueden publicar comentarios en las publicaciones del blog. Los comentarios pueden ser eliminados por el usuario que los creó y el usuario administrador.

## ⚔️ Práctica de código legible aplicadas ⚔️

### Espaciado entre bloques de código
Al desarrollar es importante que se use una correcta separación entre líneas, algo que será de gran utilidad para identificar de forma rápida bloques (o conjuntos) de código, compuestos de líneas que deberían estar relacionadas entre sí por una dependencia funcional. Debemos tener en cuenta que para el cerebro humano es más fácil recordar y aprender ideas si estas están asociadas a esquemas o formas visuales y la agrupación de líneas genera formas que, junto con el hecho anterior, de forma inconsciente facilita la comprensión del código al lector.
```python
SECRET_KEY = os.urandom(32)
app = Flask(__name__)
app.config['SECRET_KEY'] = SECRET_KEY

ckeditor = CKEditor(app)
Bootstrap(app)
gravatar = Gravatar(app, size=100, rating='g', default='retro', force_default=False, force_lower=False, use_ssl=False, base_url=None)
```

### Uso correcto de tabulaciones
Al igual que sucede con los espacios entre bloques de código, debido al mismo principio de generación de «formas», el uso correcto de tabulaciones facilita la comprensión del código al lector. En este caso, cabe resaltar el ejemplo de Python, un lenguaje que en vez de usar usa llaves para delimitar contextos usa las tabulaciones.
```python
def admin_only(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if current_user.id != 1:
            return abort(403)
        return f(*args, **kwargs)
    return decorated_function
```

### Evitar anidaciones
Es evidente que un código con muchas anidaciones resulta complejo de seguir y es por eso que siempre se debe intentar, dentro de lo posible, eliminar niveles de anidación. Veamos algunos ejemplos de cómo eliminar fácilmente anidaciones.
```python
@app.route('/register', methods=["GET", "POST"])
def register():
    form = RegisterForm()
    if form.validate_on_submit():

        if User.query.filter_by(email=form.email.data).first():
            print(User.query.filter_by(email=form.email.data).first())
            #User already exists
            flash("You've already signed up with that email, log in instead!")
            return redirect(url_for('login'))

        hash_and_salted_password = generate_password_hash(
            form.password.data,
            method='pbkdf2:sha256',
            salt_length=8
        )
        new_user = User(
            email=form.email.data,
            name=form.name.data,
            password=hash_and_salted_password,
        )
        db.session.add(new_user)
        db.session.commit()
        login_user(new_user)
        return redirect(url_for("get_all_posts"))

    return render_template("register.html", form=form, current_user=current_user)


```

### La extensión de las funciones
Cuando hablamos de funciones es importante tener en cuenta su extensión. Una función de 200 líneas resulta complicada de seguir por muy legible que sea su código interno. La longitud ideal y la extensión máxima de una función deben venir determinadas (aproximadamente) por la media de altura del monitor de un PC. La finalidad de esto es reducir el número de páginas que será necesario visitar para consultar las primeras líneas de una función y las últimas.
```python
@app.route("/delete/<int:post_id>")
@admin_only
def delete_post(post_id):
    post_to_delete = BlogPost.query.get(post_id)
    db.session.delete(post_to_delete)
    db.session.commit()
    return redirect(url_for('get_all_posts'))
```


### La extensión de las líneas de código
De forma similar a lo que ocurre con la longitud de las funciones, la extensión de las líneas de código debe venir determinada por el ancho del monitor. Una línea de código debería poder verse entera sin necesidad de usar el scroll. En caso que la línea no quepa por muchos caracteres es recomendable partirla en los fragmentos que sean necesarios.
```python
@app.route("/delete/<int:post_id>")
@admin_only
def delete_post(post_id):
    post_to_delete = BlogPost.query.get(post_id)
    db.session.delete(post_to_delete)
    db.session.commit()
    return redirect(url_for('get_all_posts'))


if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

## ⚔️ Estilos de Programación aplicados ⚔️
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

### Objetos e interacción de objetos
El problema más grande se descompone en cosas que tienen sentido para el dominio del problema.
Ejemplo: Para la manipulación de los datos se requieren un enfoque orientado a Objetos.

```python
class Comment(db.Model):
    __tablename__ = "comments"
    id = db.Column(db.Integer, primary_key=True)
    post_id = db.Column(db.Integer, db.ForeignKey("blog_posts.id"))
    author_id = db.Column(db.Integer, db.ForeignKey("users.id"))
    parent_post = relationship("BlogPost", back_populates="comments")
    comment_author = relationship("User", back_populates="comments")
    text = db.Column(db.Text, nullable=False)
db.create_all()
```

## ⚔️ Principios SOLID aplicados ⚔️
SOLID es un acrónimo acuñado por Robert C.Martin en el cual se representan los cinco principios básicos de la programación orientada a objetos. La intención de seguir estos principios es eliminar malos diseños, evitar la refactorización y construir un código más eficiente y fácil de mantener. 

### Single Responsability Principle (SRP) (Principio de responsabilidad única) 
Este principio establece que un componente o clase debe tener una responsabilidad única, sencilla y concreta. Esto simplifica el código al evitar que existan clases que cumplan con múltiples funciones, las cuales son difíciles de memorizar y muchas veces significan una pérdida de tiempo buscando qué parte del código hace qué función. 
```python
@app.route('/')
def get_all_posts():
    posts = BlogPost.query.all()
    return render_template("index.html", all_posts=posts, current_user=current_user)
```
### Dependency Inversion Principle (DIP) (Principio de inversión de dependencias) 
Este principio establece que los módulos de alto nivel no deben de depender de los de bajo nivel. En ambos casos deben depender de las abstracciones. Alto nivel se refiere a operaciones cuya naturaleza es más amplia o abarca un contexto más general y bajo nivel son componentes individuales más específicos.  
```python
from forms import LoginForm, RegisterForm, CreatePostForm, CommentForm
```
### Interface Segretation Principle (ISP) (Principio de segregación de interfaz) 
Este principio establece que los clientes no deben ser forzados a depender de interfaces que no utilizan. Es importante que cada clase implemente las interfaces que va a utilizar. De este modo, agregar nuevas funcionalidades o modificar las existentes será más fácil. 
```python
@app.route('/')
def get_all_posts():
    posts = BlogPost.query.all()
    return render_template("index.html", all_posts=posts, current_user=current_user)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

## ⚔️ Conceptos DDD aplicados ⚔️
* Repositories. Para la manipulación de datos, estos presentan un ciclo de vida que se gestiona mediante la herramienta ORM sqlalchemy
* Entities. El lenguaje de programación orientada a objetos nos permite manipular los datos a un nivel de abstración de Entidades.
* Modules. El sistema presenta modularización de sus funcionalidades para poder gestionar los recursos de forma eficiente.

## ⚔️ Estilo de Arquitectura Monolitico ⚔️
Una arquitectura monolítica es el modelo unificado tradicional para el diseño de un programa de software. Monolítico, en este contexto, significa "compuesto todo en una sola pieza". Según el diccionario de Cambridge, el adjetivo monolítico también significa tanto "demasiado grande" como "no se puede cambiar". Lo aplicamos al utilizar todo nuestro programa principal en un solo documento python.

```python
from flask import Flask, render_template, redirect, url_for, flash, abort
from flask_bootstrap import Bootstrap
from flask_ckeditor import CKEditor
from datetime import date
from functools import wraps
from werkzeug.security import generate_password_hash, check_password_hash
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.orm import relationship
from flask_login import UserMixin, login_user, LoginManager, login_required, current_user, logout_user
from forms import LoginForm, RegisterForm, CreatePostForm, CommentForm
from flask_gravatar import Gravatar
import os

SECRET_KEY = os.urandom(32)
app = Flask(__name__)
app.config['SECRET_KEY'] = SECRET_KEY

ckeditor = CKEditor(app)
Bootstrap(app)
gravatar = Gravatar(app, size=100, rating='g', default='retro', force_default=False, force_lower=False, use_ssl=False, base_url=None)

##CONNECT TO DB
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///blog.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)
login_manager = LoginManager()
login_manager.init_app(app)


@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))


##CONFIGURE TABLE
class User(UserMixin, db.Model):
    __tablename__ = "users"
    id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(100), unique=True)
    password = db.Column(db.String(100))
    name = db.Column(db.String(100))
    posts = relationship("BlogPost", back_populates="author")
    comments = relationship("Comment", back_populates="comment_author")


class BlogPost(db.Model):
    __tablename__ = "blog_posts"
    id = db.Column(db.Integer, primary_key=True)
    author_id = db.Column(db.Integer, db.ForeignKey("users.id"))
    author = relationship("User", back_populates="posts")
    title = db.Column(db.String(250), unique=True, nullable=False)
    subtitle = db.Column(db.String(250), nullable=False)
    date = db.Column(db.String(250), nullable=False)
    body = db.Column(db.Text, nullable=False)
    img_url = db.Column(db.String(250), nullable=False)
    comments = relationship("Comment", back_populates="parent_post")


class Comment(db.Model):
    __tablename__ = "comments"
    id = db.Column(db.Integer, primary_key=True)
    post_id = db.Column(db.Integer, db.ForeignKey("blog_posts.id"))
    author_id = db.Column(db.Integer, db.ForeignKey("users.id"))
    parent_post = relationship("BlogPost", back_populates="comments")
    comment_author = relationship("User", back_populates="comments")
    text = db.Column(db.Text, nullable=False)
db.create_all()


def admin_only(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if current_user.id != 1:
            return abort(403)
        return f(*args, **kwargs)
    return decorated_function


@app.route('/')
def get_all_posts():
    posts = BlogPost.query.all()
    return render_template("index.html", all_posts=posts, current_user=current_user)


@app.route('/register', methods=["GET", "POST"])
def register():
    form = RegisterForm()
    if form.validate_on_submit():

        if User.query.filter_by(email=form.email.data).first():
            print(User.query.filter_by(email=form.email.data).first())
            #User already exists
            flash("You've already signed up with that email, log in instead!")
            return redirect(url_for('login'))

        hash_and_salted_password = generate_password_hash(
            form.password.data,
            method='pbkdf2:sha256',
            salt_length=8
        )
        new_user = User(
            email=form.email.data,
            name=form.name.data,
            password=hash_and_salted_password,
        )
        db.session.add(new_user)
        db.session.commit()
        login_user(new_user)
        return redirect(url_for("get_all_posts"))

    return render_template("register.html", form=form, current_user=current_user)


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


@app.route('/logout')
def logout():
    logout_user()
    return redirect(url_for('get_all_posts'))


@app.route("/post/<int:post_id>", methods=["GET", "POST"])
def show_post(post_id):
    form = CommentForm()
    requested_post = BlogPost.query.get(post_id)

    if form.validate_on_submit():
        if not current_user.is_authenticated:
            flash("You need to login or register to comment.")
            return redirect(url_for("login"))

        new_comment = Comment(
            text=form.comment_text.data,
            comment_author=current_user,
            parent_post=requested_post
        )
        db.session.add(new_comment)
        db.session.commit()

    return render_template("post.html", post=requested_post, form=form, current_user=current_user)


@app.route("/about")
def about():
    return render_template("about.html", current_user=current_user)


@app.route("/contact")
def contact():
    return render_template("contact.html", current_user=current_user)


@app.route("/new-post", methods=["GET", "POST"])
@admin_only
def add_new_post():
    form = CreatePostForm()
    if form.validate_on_submit():
        new_post = BlogPost(
            title=form.title.data,
            subtitle=form.subtitle.data,
            body=form.body.data,
            img_url=form.img_url.data,
            author=current_user,
            date=date.today().strftime("%B %d, %Y")
        )
        db.session.add(new_post)
        db.session.commit()
        return redirect(url_for("get_all_posts"))

    return render_template("make-post.html", form=form, current_user=current_user)


@app.route("/edit-post/<int:post_id>", methods=["GET", "POST"])
@admin_only
def edit_post(post_id):
    post = BlogPost.query.get(post_id)
    edit_form = CreatePostForm(
        title=post.title,
        subtitle=post.subtitle,
        img_url=post.img_url,
        author=current_user,
        body=post.body
    )
    if edit_form.validate_on_submit():
        post.title = edit_form.title.data
        post.subtitle = edit_form.subtitle.data
        post.img_url = edit_form.img_url.data
        post.body = edit_form.body.data
        db.session.commit()
        return redirect(url_for("show_post", post_id=post.id))

    return render_template("make-post.html", form=edit_form, is_edit=True, current_user=current_user)


@app.route("/delete/<int:post_id>")
@admin_only
def delete_post(post_id):
    post_to_delete = BlogPost.query.get(post_id)
    db.session.delete(post_to_delete)
    db.session.commit()
    return redirect(url_for('get_all_posts'))


if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)

```




