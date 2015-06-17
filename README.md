# Proyecto_Factura-Final-
Este sera el esqueleto para el proyecto "Registro_Factura"
namespace Modelo
{
    //Interfaz para Factura
    public interface ICalculable
    {
        void calcularNeto();//Los metodos los implementara para caulcular tanto lo valores neto e
        void calcularIVA(); //IVA
    }
    public class Factura: ICalculable
    {
        /// <summary>
        /// primeraCantidad,segundaCantidad,terceraCantidad: cantdad que querra el usurio de un 
        /// mismo producto
        /// </summary>
        public int primeraCantidad { get; set; }
        public int segundaCantidad { get; set; }
        public int terceraCantidad { get; set; }
        
        //Donde se encuentran los prudctos en forma de ennumerador
        #region Productos
        private Producto prmProducto;

        public Producto primerProducto
        {
            get { return prmProducto; }
            set { prmProducto = value; }
        }

        private Producto sgnProducto;

        public Producto segundoProducto
        {
            get { return sgnProducto; }
            set { sgnProducto = value; }
        }

        private Producto tProducto;

        public Producto tercerProducto
        {
            get { return tProducto; }
            set { tProducto = value; }
        } 
        #endregion
        

        public DateTime Fecha { get; set; }//Fecha de la factura

        public Persona Persona { get; set; }//Asociacion con la clase Persona

        private int intNumFactura; //Num. de la factura 

        public int numFactura
        {
            get { return intNumFactura; }
            set { intNumFactura = value; }
        }
        
        public int calcularTotal(int intCantidad, Producto producto) //Metodo para calcular el valor monetario de unproducto,
        {                                                            //ingresanto un parametro int de la cantidad de un                                                                           //objeto especifico
            int calculo;                                             //y el valor del producto.
            int valor = (int)producto;
            calculo = valor * intCantidad;
            return calculo;
        }

        public void calcularNeto()
        {

            throw new NotImplementedException();
        }

        public void calcularIVA()
        {
            throw new NotImplementedException();
        }
    }
    public class Persona
    {
        //Metodo sin parametros de entrada
        public Persona() { }

        //Metodo con parametros de entrada
        public Persona(string strParNombre)
        {
            Nombre = strParNombre;
        }

        private string strNombre;//Nombre de la persona, tanto para el cliente como para el Vendedor

        public string Nombre
        {
            get { return strNombre; }
            set { strNombre = value; }
        }

        //Metodo imprimir que mostrara la informacion de la persona, se extendera hacia el cliente.
        public virtual string imprimir()
        {
            return "Nombre: "+Nombre;
        }
    }
    public class Vendedor:Persona//extiende de la clase persona
    {
        //Metodo sin parametros de entrada
        public Vendedor() { }

        //Metodo con parametros de entrada
        public Vendedor(string intParCodigo, string strBaseNombre): base(strBaseNombre)
        {
            Codigo = intParCodigo;
        }

        protected string intCodigo;//Contraseña del vendedor

        public string Codigo
        {
            get { return intCodigo; }
            set { intCodigo = value; }
        }
    }
    
    /// <summary>
    /// extiende de la clase persona
    /// </summary>
    public class Cliente:Persona 
    {
        
        /// <summary>
        /// Rut del cliente
        /// </summary>
        private string strRut;

        public string Rut
        {
            get { return strRut; }
            set { strRut = value; }
        }
        //Telefono fijo del cliente
        private int intTelefono;

        public int Telefono
        {
            get { return intTelefono; }
            set { intTelefono = value; }
        }
        
        //Telefono movil del cliente
        public int Movil { get; set; }
        //Direccion de cliente
        public string Direccion { get; set; }
        
        //La comuna del cliente, que depende de enumeradores
        private Comuna itComuna;

        public Comuna comuna
        {
            get { return itComuna; }
            set { itComuna = value; }
        }

        public override string imprimir() //metodo que sobrescribe el metodo anterior de la clase persona, y agrega nuea                                              //informacion
        {
            return base.imprimir()+
                "\nRut: "+Rut+
                "\nTelefono de contacto: "+
                "\nFijo: "+Telefono+
                "\nMovil: "+Movil+
                "Direccion: "+Direccion+","+comuna;
        }
    }
    
    public class RegistroFactura
    {
        private List<Factura> facturas;

        public RegistroFactura()
        {
            facturas = new List<Factura>();
        }

        public Boolean agregarFactura(Factura nvaFactura)
        {
            facturas.Add(nvaFactura);
            return true;
        }

        public Boolean eliminarFactura(int numeroFactura)
        {
            foreach (var item in facturas)
            {
                if (item.numFactura==numeroFactura)
                {
                    facturas.Remove(item);
                    return true;
                }
            }
            return false;
        }
    }
    
    public partial class MainWindow : Window
    {
        List<Vendedor> vnt = new List<Vendedor>();
        public MainWindow()
        {
            InitializeComponent();
            vnt.Add(new Vendedor("2002", "Pedro"));
            vnt.Add(new Vendedor("3003", "Juan"));
        }

        private void btnLogin_Click(object sender, RoutedEventArgs e)
        {
            foreach (var item in vnt)
            {
                if (txbNombre.Text == item.Nombre && pwbClave.Password == item.Codigo)
                {
                    ventanGuardarProducto nuevaVentana = new ventanGuardarProducto(item);
                    nuevaVentana.Show();
                    this.Close();
                    break;
                }
                else if (txbNombre.Text != item.Nombre && pwbClave.Password != item.Codigo)
                {
                    continue;
                }
                else
                {
                    MessageBox.Show("Nombre de usuario o contraseña mal escrita");
                }
            }
        }
    }
    
    public partial class ventanGuardarProducto : Window
    {
        RegistroFactura reg = new RegistroFactura();
        Factura fc;
        
        public ventanGuardarProducto(object nuevoobjeto)
        {
            InitializeComponent();
            if (nuevoobjeto is Vendedor)
            {
                Vendedor vnt = (Vendedor)nuevoobjeto;
                txbNombreVendedor.Text = vnt.Nombre;
            }
            cbComuna.ItemsSource = Enum.GetValues(typeof(Comuna));
            cbProductoUno.ItemsSource = Enum.GetValues(typeof(Producto));
            cbProductoDos.ItemsSource = Enum.GetValues(typeof(Producto));
            cbProductoTres.ItemsSource = Enum.GetValues(typeof(Producto));
        }

        private void btnGuardar_Click(object sender, RoutedEventArgs e)
        {
            try
            {
                fc = new Factura();
              /*  Cliente ct = new Cliente();
                fc.Persona = ct;*/

                //fc.Persona.Nombre = txbNombreCliente.Text;
                if(fc.Persona is Cliente)
                {
                    Cliente cl = (Cliente)fc.Persona;
                    cl.Nombre = txbNombreCliente.Text;
                    cl.Rut = txbRut.Text;
                    cl.Telefono = int.Parse(txbNumFijo.Text);
                    cl.Movil = int.Parse(txbNumMovil.Text);
                    cl.Direccion = txbDireccion.Text;
                    cl.comuna = (Comuna)cbComuna.SelectedItem;
                }
                fc.primerProducto = (Producto)cbProductoUno.SelectedItem;
                fc.segundoProducto = (Producto)cbProductoDos.SelectedItem;
                fc.tercerProducto = (Producto)cbProductoTres.SelectedItem;
                if (fc.Persona is Vendedor)
                {
                    Vendedor vd = (Vendedor)fc.Persona;
                    vd.Nombre = txbNombreVendedor.Text;
                }
                DateTime fecha = DateTime.UtcNow;
                string soloFecha = fecha.ToString("d");
                fc.Fecha = Convert.ToDateTime(soloFecha);
                reg.agregarFactura(fc);
                MessageBox.Show("Guardado correctamente");
            }
            catch (Exception)
            {

                MessageBox.Show("");
            }
        }
    }
}
