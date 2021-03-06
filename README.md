# ПОНЯТЬ СМАРТ-КОНТРАКТ ТОКЕНА ERC-20

Один из наиболее важных стандартов смарт-контрактов в Ethereum известен как ERC-20 , который стал техническим стандартом, используемым для всех смарт-контрактов в блокчейне Ethereum для реализации взаимозаменяемых токенов.

ERC-20 определяет общий список правил, которых должны придерживаться все взаимозаменяемые токены Ethereum. Следовательно, этот стандарт токенов позволяет разработчикам всех типов точно прогнозировать, как новые токены будут работать в более крупной системе Ethereum. Это упрощает и упрощает задачи разработчиков, поскольку они могут продолжать свою работу, зная, что каждый новый проект не нужно будет переделывать каждый раз при выпуске нового токена, если токен следует правилам.

# В этом руководстве мы узнаем, как можно легко использовать наследование для организации кода. В объектно-ориентированном программировании класс - это план для создания объектов (определенной структуры данных), обеспечивающий начальные значения для состояния (переменные-члены или атрибуты) и реализации поведения (функции-члены или методы).

В Solidity контракт действует как класс. Вы можете наследовать от контракта, чтобы совместно использовать общий интерфейс (сигнализируйте другим контрактам, что ваш контракт реализует определенный набор функций, например: токены ERC20 или NFT ...).

Мы вернемся к нашему смарт-контракту Counter, чтобы создать общий интерфейс счетчика. Определим следующий договор.

pragma solidity 0.5.17;

contract Counter {
    
    //Event emitted when the value is changed
    event ValueChanged(uint256 newValue);
    
    // Private variable of type unsigned int to keep the number of counts
    uint256 private count;
    
     constructor(uint256 startValue) public {
        count = startValue;
    }
    
    // Function that will modify our counter
    function setCounter(uint256 newValue) internal {
        count = newValue;
        emit ValueChanged(count);
    }
    
    // Getter to get the count value
     function getCount() public view returns (uint256) {
        return count;
    }
    
    // The function that child contracts need to implement  as an increment or decrement
    function step() public;
    
}
Примечание:

Строка 16 : внутреннее ключевое слово для функции setCounter делает эту функцию доступной только для дочернего контракта и недоступной снаружи.

Строка 27 : пошаговая функция, которая должна быть реализована контрактами, которые унаследуют контракт Counter .

Допустим, мы хотим иметь счетчик, который действует как инкремент и всегда будет добавлять 1 к счетчику всякий раз, когда вызывается пошаговая функция.

contract IncrementCounter is Counter {
    
    constructor(uint256 startValue) Counter(startValue) public {}
    
    function step() public {
        setCounter(getCount() + 1);
    }
    
}
Вы увидите это:

Строка 1 : мы используем ключевое слово is, чтобы сообщить компилятору, что IncrementCounter наследуется от контракта Counter .

Строка 3: если у родительского контракта есть конструктор, нам нужно будет вызвать его из дочернего конструктора с соответствующими параметрами.

Строка 6: мы реализуем пошаговую функцию, в которой мы сообщаем родительскому контракту, какое будет новое значение. Вызов setCounter установит новое значение, а также выдаст событие журнала.

Теперь, когда у нас есть IncrementCounter, мы можем сделать то же самое, например, с CountDown100 . Он напрямую устанавливает начальное значение на 100 и будет уменьшать счетчик, когда мы вызываем пошаговую функцию.

contract CountDown100 is Counter {
    
    constructor() Counter(100) public {}
    
    function step() public {
        setCounter(getCount() - 1);
    }
    
}
В случае, когда вы определяете контракт без внутренней логики (просто определение функции или события), он называется интерфейсом . Вы можете, например, посмотреть на этот упрощенный интерфейс токена ERC20:

interface IERC20 {
  
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
}
