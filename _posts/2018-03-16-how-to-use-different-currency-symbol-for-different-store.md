# Magento如何修改为不同的store view指定不同的货币符号

​    Magento 默认后台修改货币符号 System->Manage Currency->Symbols 只能全局修改货币符号，不能基于每个website或store分别设置。下面的代码可以实现给不同的store设置不同的currency symbol的方法。




	error_reporting(E_ALL | E_STRICT);

	define('MAGENTO_ROOT', getcwd());
	$mageFilename = MAGENTO_ROOT . '/app/Mage.php';
	require_once $mageFilename;
	umask(0);
	Mage::init();

	$storeCode = 'your_store_code';
	$store = Mage::getModel('core/store')->load($storeCode);
	$symbol = 'CN¥';
	$currencies = unserialize(Mage::getStoreConfig('currency/options/customsymbol', $store->getId())); 
	$currencies['CNY'] = $symbol; 
	Mage::getModel('core/config')->saveConfig('currency/options/customsymbol', serialize($currencies), 'stores', $store->getId() );
	echo $code . ' updated to CN¥' . '<br>';


在网站根目录下执行这个文件，刷新cache后即可生效。