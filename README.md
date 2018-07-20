# �� WPF ��ʵ�ֲ��ʽ���� (MEF + MvvmLightLibsStd10)

> ֮ǰһֱʹ�� MvvmLight ������ص� WPF �������������ʽ���������� 2 ��ǰ�Ӵ��� Prism ��ܺ������ʵ���������� Prism ����̫�أ���̫�ʺ��������ţ��ҵ���Ҳ������˺þ�һ��ʱ�䡣
�������ֿ���һ�� MEF ����ض����������������˳�����ˣ�����д��һ�����װ�� Demo ��һ����


## ʾ������

* ͨ�� IOC �� DI ��ʽ�������ģ�鵽���Ͷ�̬����
* ͨ��ʹ�� MvvmLight �� MessengerInstance ʵ��ģ���ͨ��

## �ؼ�ʾ������

* ������Sample

```C#
    public partial class MainWindow : Window
    {
        private CompositionContainer container = null;

        public MainWindow()
        {
            InitializeComponent();
            var dir = new DirectoryInfo(Path.Combine(Environment.CurrentDirectory, "Plugins"));
            if (dir.Exists)
            {
                var catalog = new DirectoryCatalog(dir.FullName, "Sample.*.dll");
                container = new CompositionContainer(catalog);
                container.ComposeParts(this);

                var list = container.GetExportedValues<IView>();
                foreach (var item in list)
                {
                    var attr = item.GetType().GetCustomAttribute<ExportMetadataAttribute>();
                    this.tab.Items.Add(new TabItem() { Header = attr.Value, Content = item });
                }
            }
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            container?.Dispose();
            base.OnClosing(e);
        }
    }
```

* ��ģ�� 1��Sample.Plugin1

```C#
    [Export(typeof(IView))]
    [ExportMetadata("name", "Plugin1")]
    public partial class MainView : UserControl, IView
    {
        [ImportingConstructor]
        public MainView([Import("DataService")]IService service)
        {
            this.DataContext = new MainViewModel(service);
            InitializeComponent();
        }
    }
```


* ��ģ�� 2��Sample.Plugin2

```C#
    [Export(typeof(IView))]
    [ExportMetadata("name", "Plugin2")]
    public partial class MainView : UserControl, IView
    {
        public MainView()
        {
            this.DataContext = new MainViewModel();
            InitializeComponent();
        }
    }
```


# �ο���

* [Building Hello MEF �C Part II �C Metadata and why being Lazy is a good thing.](https://blogs.msdn.microsoft.com/gblock/2009/12/04/building-hello-mef-part-ii-metadata-and-why-being-lazy-is-a-good-thing/)
* [��WPF������MEFʵ�ִ��ڵĶ�̬��չ](https://www.cnblogs.com/tcjiaan/p/5844619.html)