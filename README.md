# Iniciar-Sesion-Registrarse

class Usuario:
    def __init__(self, nombre, correo, contraseña):
        self.nombre = nombre
        self.correo = correo
        self.contraseña = contraseña

class ControlAcceso:
    def __init__(self):
        self.usuarios = []

    def registrar_usuario(self, nombre, correo, contraseña):
        if self.buscar_usuario(correo):
            return False, "❌ El correo ya está registrado."
        nuevo_usuario = Usuario(nombre, correo, contraseña)
        self.usuarios.append(nuevo_usuario)
        return True, "✅ Usuario registrado correctamente."

    def iniciar_sesion(self, correo, contraseña):
        usuario = self.buscar_usuario(correo)
        if usuario and usuario.contraseña == contraseña:
            return True, f"✅ Bienvenido, {usuario.nombre}."
        return False, "❌ Credenciales incorrectas."

    def buscar_usuario(self, correo):
        for usuario in self.usuarios:
            if usuario.correo == correo:
                return usuario
        return None


class UIAcceso:
    def __init__(self, controlador):
        self.controlador = controlador

    def mostrar_menu(self):
        while True:
            print("\n--- Menú de Acceso ---")
            print("1. Registrarse")
            print("2. Iniciar sesión")
            print("3. Salir")
            opcion = input("Seleccione una opción: ")

            if opcion == "1":
                self.registrarse()
            elif opcion == "2":
                self.iniciar_sesion()
            elif opcion == "3":
                print("👋 Saliendo del sistema...")
                break
            else:
                print("⚠️ Opción inválida.")

    def registrarse(self):
        print("\n📋 Registro de Usuario")
        nombre = input("Nombre: ")
        correo = input("Correo electrónico: ")
        contraseña = input("Contraseña: ")
        ok, mensaje = self.controlador.registrar_usuario(nombre, correo, contraseña)
        print(mensaje)

    def iniciar_sesion(self):
        print("\n🔐 Inicio de Sesión")
        correo = input("Correo electrónico: ")
        contraseña = input("Contraseña: ")
        ok, mensaje = self.controlador.iniciar_sesion(correo, contraseña)
        print(mensaje)


if __name__ == "__main__":
    controlador = ControlAcceso()
    interfaz = UIAcceso(controlador)
    interfaz.mostrar_menu()
