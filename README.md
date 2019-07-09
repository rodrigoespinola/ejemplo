# ejemplo
muestra de algo que he desarrollado

using System;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data;
using System.Data.Sql;
using System.Data.SqlClient;
using System.Drawing;
using System.Text;
using System.Runtime.Serialization;
using System.Runtime.Serialization.Formatters.Binary;

namespace cargar_fotos_informe_tecnico
{
    public class conexion
    {
        SqlConnection cn;
        DataTable dt;
        SqlDataAdapter da;
        
        public conexion() //constructor de la clase (siempre debe tener el mismo nombre de la clase ;D)
            {
                cn = new SqlConnection("Data Source=200.111.102.206;Initial Catalog=BODEGAPRUEBA;User ID=sa;Password=losandes147+*");
            }
            public String insertar_tabla(byte[] fotografia,String comentario )
            {
                try
                {
                    using (SqlCommand insercion = new SqlCommand("Insert Into prueba_digitalizacion (imagen,comentario) values (@imagen,@comentario)", cn))
                    {
                        
                        insercion.Parameters.Add("@imagen", SqlDbType.Image).Value = fotografia;
                        insercion.Parameters.Add("@comentario", SqlDbType.Text).Value = comentario;
                        cn.Open();
                        insercion.ExecuteNonQuery();
                        cn.Close();
                    }
                    return "ok";
                }
                catch (Exception ex)
                {
                    return ex.ToString();
                }
                finally 
                {
                    cn.Close();
                }
            }
            public List<String> select_generico(string consulta)
            {
                try
                {
                    List<String> retorno_consulta = new List<string>();
                    da = new SqlDataAdapter(consulta, cn);
                    dt = new DataTable();
                    da.Fill(dt);

                    for (int j = 0; j < dt.Rows.Count; j++)
                    {
                        for (int i = 0; i < dt.Columns.Count; i++)
                        {
                            retorno_consulta.Add(dt.Rows[j][i].ToString());
                        }
                    }

                    return retorno_consulta;
                }
                catch (Exception ex)
                {
                    List<String> retorno_consulta = new List<string>();
                    retorno_consulta.Add(ex.Message);
                    return retorno_consulta;
                }
                finally
                {
                    cn.Close();
                }

            }
            public void buscar_un_campo_discreto(String consulta, )
            {
                try
                {
                    da = new SqlDataAdapter(consulta, cn);
                    dt = new DataTable();
                    da.Fill(dt);
                    //campo.Text = dt.Rows[0][0].ToString();
                }
                catch
                {
                }
                finally
                {
                    cn.Close();
                }
            }
    }
}
