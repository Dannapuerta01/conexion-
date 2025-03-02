const express = require("express");
const cors = require("cors");

const mysql = require("mysql");
const app = express();
app.use(cors());
app.use(express.json());
const connection = mysql.createConnection({
  host: "db4free.net",
  user: "estudiantesweb",
  password: "admin12345",
  database: "cursoweb",
  port: 3306,
});

connection.connect((err) => {
  if (err) throw err;
  console.log("Conectado a mysql");
});

app.post("/registro", (req, res) => {
  const { name, last_name, identification, email, phone } = req.body;
  connection.query(
    "INSERT INTO student(name,last_name, identification, email, phone) VALUES (?, ?, ?, ?, ?)",
    [name, last_name, identification, email, phone],
    (err, result) => {
      res.json("Registro listo");
    },
  );
});

app.get("/", (req, res) => {
  connection.query("select * from student;", (err, result) => {
    res.send(result);
  });
});

app.put("/studentU/:id", (req, res) => {
  const { id } = req.params;
  const { email } = req.body;

  connection.query(
    "UPDATE student SET email=? WHERE id = ?",
    [email, id],
    (err, result) => {
      if (err) console.log(err);
      res.json({ message: "Actualizacion completa", result });
    },
  );
});

app.listen(3000, () => {
  console.log("Servidor iniciado");
});
