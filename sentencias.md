# SENTENCIAS

Voy a dar ciertos ejemplos de consultas para saber diferencias y como funcionan:
  
  SELECT
      ClienteID,
      Producto,
      TotalGastado, /* esta fila sale del FROM usando el GROUP BY */
      ROW_NUMBER() OVER (
          PARTITION BY ClienteID /* Hace que se particione la tabla, en este caso usando el id del cliente, es decir ROW_NUMBER va a enumerar pero cuando se cambie de id de cliente vuelve a empezar */
          ORDER BY TotalGastado DESC /* Se va a ordenar de mayor a menor a partir del total gastado en ese producto por el cliente, ya que esta columna sale del group by */
      ) AS Rn
  FROM (
      SELECT 
          ClienteID,
          Producto,
          SUM(Monto) AS TotalGastado
      FROM Sales
      GROUP BY ClienteID, Producto  /* Encontrara todas las dilas con el mismo ClienteID y producto para sumar el monto y dar el total gastado de ese cliente en ese determinado producto */
  ) AS agrupado;  /* Se peude quitar el AS */
