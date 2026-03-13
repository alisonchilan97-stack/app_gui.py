# app_gui.py
import tkinter as tk
from tkinter import ttk, messagebox
import json

# -----------------------------
# Archivo donde se guardarán datos
# -----------------------------
ARCHIVO = "datos.json"

# -----------------------------
# Cargar datos guardados
# -----------------------------
def cargar_datos():
    try:
        with open(ARCHIVO, "r") as f:
            datos = json.load(f)
            for dato in datos:
                tabla.insert("", tk.END, values=(dato,))
    except:
        pass


# -----------------------------
# Guardar datos en archivo
# -----------------------------
def guardar_datos():
    datos = []
    for item in tabla.get_children():
        datos.append(tabla.item(item)["values"][0])

    with open(ARCHIVO, "w") as f:
        json.dump(datos, f)


# -----------------------------
# Agregar dato
# -----------------------------
def agregar():
    dato = entrada.get()

    if dato == "":
        messagebox.showwarning("Aviso", "Ingrese un dato")
        return

    tabla.insert("", tk.END, values=(dato,))
    entrada.delete(0, tk.END)

    guardar_datos()


# -----------------------------
# Eliminar dato seleccionado
# -----------------------------
def eliminar():
    seleccionado = tabla.selection()

    if not seleccionado:
        messagebox.showwarning("Aviso", "Seleccione un dato para eliminar")
        return

    for item in seleccionado:
        tabla.delete(item)

    guardar_datos()


# -----------------------------
# Limpiar campo de texto
# -----------------------------
def limpiar():
    entrada.delete(0, tk.END)


# -----------------------------
# Ventana principal
# -----------------------------
ventana = tk.Tk()
ventana.title("Gestor de Datos")
ventana.geometry("450x350")
ventana.config(bg="#f0f0f0")

# -----------------------------
# Título
# -----------------------------
titulo = tk.Label(
    ventana,
    text="Gestor de Información",
    font=("Arial", 16, "bold"),
    bg="#f0f0f0"
)
titulo.pack(pady=10)

# -----------------------------
# Frame entrada
# -----------------------------
frame_entrada = tk.Frame(ventana, bg="#f0f0f0")
frame_entrada.pack(pady=5)

label = tk.Label(frame_entrada, text="Ingrese dato:", bg="#f0f0f0")
label.grid(row=0, column=0, padx=5)

entrada = tk.Entry(frame_entrada, width=25)
entrada.grid(row=0, column=1, padx=5)

# -----------------------------
# Botones
# -----------------------------
frame_botones = tk.Frame(ventana, bg="#f0f0f0")
frame_botones.pack(pady=10)

btn_agregar = tk.Button(frame_botones, text="Agregar", width=10, bg="#4CAF50", fg="white", command=agregar)
btn_agregar.grid(row=0, column=0, padx=5)

btn_eliminar = tk.Button(frame_botones, text="Eliminar", width=10, bg="#f44336", fg="white", command=eliminar)
btn_eliminar.grid(row=0, column=1, padx=5)

btn_limpiar = tk.Button(frame_botones, text="Limpiar", width=10, bg="#2196F3", fg="white", command=limpiar)
btn_limpiar.grid(row=0, column=2, padx=5)

# -----------------------------
# Tabla de datos
# -----------------------------
tabla = ttk.Treeview(ventana, columns=("Dato"), show="headings", height=10)
tabla.heading("Dato", text="Datos ingresados")
tabla.pack(pady=10)

# -----------------------------
# Cargar datos al iniciar
# -----------------------------
cargar_datos()

# -----------------------------
# Ejecutar programa

/**************************************
La aplicación fue desarrollada utilizando la librería Tkinter de Python, la cual permite crear interfaces gráficas de usuario. El objetivo del programa es permitir que los usuarios ingresen información, visualizarla en una tabla y gestionar esos datos de manera sencilla.

La interfaz cuenta con una ventana principal que incluye un título, un campo de texto para ingresar datos, botones para realizar acciones y una tabla donde se muestran los datos agregados. El usuario puede escribir información en el campo de texto y presionar el botón Agregar, lo que permite que el dato se muestre inmediatamente en la tabla.

También se incluye un botón Eliminar, que permite borrar un dato seleccionado de la tabla, y un botón Limpiar, que borra el texto escrito en el campo de entrada. La aplicación responde a los eventos generados por el usuario, como los clics en los botones, ejecutando las funciones correspondientes.

Además, el programa guarda automáticamente la información en un archivo externo, lo que permite que los datos permanezcan guardados incluso después de cerrar la aplicación. Cuando el programa se vuelve a abrir, los datos almacenados se cargan nuevamente y se muestran en la tabla.


# -----------------------------
ventana.mainloop()
