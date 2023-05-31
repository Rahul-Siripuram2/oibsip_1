# oibsip_1
<!== This code is Display the Calculator ==>
<!==This code is Done by Siripuram Rahul ==>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#000000" />
    <meta name="description" content="Web site created using create-react-app" />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <title>React App</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script crossorigin src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
    <title>Document</title>
    <style>
       
@import 'https://fonts.googleapis.com/css?family=Share+Tech+Mono';
@font-face {
  font-family: 'Digital';
  src: url('//db.onlinewebfonts.com/t/8e22783d707ad140bffe18b2a3812529.eot');
  src: url('//db.onlinewebfonts.com/t/8e22783d707ad140bffe18b2a3812529.eot?#iefix')
      format('embedded-opentype'),
    url('//db.onlinewebfonts.com/t/8e22783d707ad140bffe18b2a3812529.woff2')
      format('woff2'),
    url('//db.onlinewebfonts.com/t/8e22783d707ad140bffe18b2a3812529.woff')
      format('woff'),
    url('//db.onlinewebfonts.com/t/8e22783d707ad140bffe18b2a3812529.ttf')
      format('truetype'),
    url('//db.onlinewebfonts.com/t/8e22783d707ad140bffe18b2a3812529.svg#Digital-7')
      format('svg');
}

#app {
  height: 780px;
  display: flex;
  align-items: center;
  justify-content: center;
}

body {
  background: linear-gradient(rgb(99, 228, 95),rgb(22, 173, 67),rgb(168, 223, 67),rgb(140, 212, 23));
  -webkit-touch-callout: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -webkit-user-select: none;
  user-select: none;
  cursor: default;
}

.calculator {
  border: 4px solid black;
  padding: 5px;
  background: linear-gradient(to top,rgb(120, 121, 126),rgb(187, 187, 196)) ;
  width: 320px;
  position: relative;
}

.formulaScreen {
  min-height: 30px;
  font-family: digital;
  font-size: 30px;
  color: purple;
  text-align: right;
  vertical-align: text-top;
  line-height: 20px;
  overflow-wrap: break-word;
  word-wrap: break-word;
}

.outputScreen {
  font-size: 29px;
  font-family: digital;
  color: white;
  text-align: right;
  line-height: 35px;
}

button {
  position: relative;
  height: 65px;
  width: 80px;
  color: black;
  outline: 2px solid black;
  border: none;
  background:linear-gradient(to top,rgb(148, 149, 221),rgb(49, 59, 185));
  font-family: Share Tech Mono, monospace;
  font-size: 30px;
  cursor: default;
  &:hover {
    color: aqua;
    background-color: blueviolet;
    outline: 0.05em solid yellow;
    z-index: 3;
  }
}

.jumbo {
  width: 160px;
}

.author {
  text-align: center;
  font-family: Share Tech Mono, sans;
  margin-top: 15px;
 }
 .author a{
    text-decoration: none;
    color: #4b8bca;
    line-height: 46px;
  }
</style>
</head>
<body>
    <div id="app"></div>
    <script type="text/babel">
        const projectName = 'javascript-calculator';

// VARS:
const isOperator = /[x/+‑]/,
  endsWithOperator = /[x+‑/]$/,
  endsWithNegativeSign = /\d[x/+‑]{1}‑$/,
  clearStyle = { background: '#ac3939' },
  operatorStyle = { background: '#666666' },
  equalsStyle = {
    background: 'linear-gradient(to top,rgb(204, 146, 204),rgb(167, 46, 167))',
    position: 'absolute',
    height: 130,
    bottom: 5
  };

// COMPONENTS:
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      currentVal: '0',
      prevVal: '0',
      formula: '',
      currentSign: 'pos',
      lastClicked: ''
    };
    this.maxDigitWarning = this.maxDigitWarning.bind(this);
    this.handleOperators = this.handleOperators.bind(this);
    this.handleEvaluate = this.handleEvaluate.bind(this);
    this.initialize = this.initialize.bind(this);
    this.handleDecimal = this.handleDecimal.bind(this);
    this.handleNumbers = this.handleNumbers.bind(this);
  }

  maxDigitWarning() {
    this.setState({
      currentVal: 'Digit Limit Met',
      prevVal: this.state.currentVal
    });
    setTimeout(() => this.setState({ currentVal: this.state.prevVal }), 1000);
  }

  handleEvaluate() {
    if (!this.state.currentVal.includes('Limit')) {
      let expression = this.state.formula;
      while (endsWithOperator.test(expression)) {
        expression = expression.slice(0, -1);
      }
      expression = expression
        .replace(/x/g, '*')
        .replace(/‑/g, '-')
        .replace('--', '+0+0+0+0+0+0+');
      let answer = Math.round(1000000000000 * eval(expression)) / 1000000000000;
      this.setState({
        currentVal: answer.toString(),
        formula:
          expression
            .replace(/\*/g, '*')
            .replace(/-/g, '‑')
            .replace('+0+0+0+0+0+0+', '‑-')
            .replace(/(x|\/|\+)‑/, '$1-')
            .replace(/^‑/, '-') +
          '=' +
          answer,
        prevVal: answer,
        evaluated: true
      });
    }
  }

  handleOperators(e) {
    if (!this.state.currentVal.includes('Limit')) {
      const value = e.target.value;
      const { formula, prevVal, evaluated } = this.state;
      this.setState({ currentVal: value, evaluated: false });
      if (evaluated) {
        this.setState({ formula: prevVal + value });
      } else if (!endsWithOperator.test(formula)) {
        this.setState({
          prevVal: formula,
          formula: formula + value
        });
      } else if (!endsWithNegativeSign.test(formula)) {
        this.setState({
          formula:
            (endsWithNegativeSign.test(formula + value) ? formula : prevVal) +
            value
        });
      } else if (value !== '‑') {
        this.setState({
          formula: prevVal + value
        });
      }
    }
  }

  handleNumbers(e) {
    if (!this.state.currentVal.includes('Limit')) {
      const { currentVal, formula, evaluated } = this.state;
      const value = e.target.value;
      this.setState({ evaluated: false });
      if (currentVal.length > 21) {
        this.maxDigitWarning();
      } else if (evaluated) {
        this.setState({
          currentVal: value,
          formula: value !== '0' ? value : ''
        });
      } else {
        this.setState({
          currentVal:
            currentVal === '0' || isOperator.test(currentVal)
              ? value
              : currentVal + value,
          formula:
            currentVal === '0' && value === '0'
              ? formula === ''
                ? value
                : formula
              : /([^.0-9]0|^0)$/.test(formula)
              ? formula.slice(0, -1) + value
              : formula + value
        });
      }
    }
  }

  handleDecimal() {
    if (this.state.evaluated === true) {
      this.setState({
        currentVal: '0.',
        formula: '0.',
        evaluated: false
      });
    } else if (
      !this.state.currentVal.includes('.') &&
      !this.state.currentVal.includes('Limit')
    ) {
      this.setState({ evaluated: false });
      if (this.state.currentVal.length > 21) {
        this.maxDigitWarning();
      } else if (
        endsWithOperator.test(this.state.formula) ||
        (this.state.currentVal === '0' && this.state.formula === '')
      ) {
        this.setState({
          currentVal: '0.',
          formula: this.state.formula + '0.'
        });
      } else {
        this.setState({
          currentVal: this.state.formula.match(/(-?\d+\.?\d*)$/)[0] + '.',
          formula: this.state.formula + '.'
        });
      }
    }
  }

  initialize() {
    this.setState({
      currentVal: '0',
      prevVal: '0',
      formula: '',
      currentSign: 'pos',
      lastClicked: '',
      evaluated: false
    });
  }

  render() {
    return (
      <div>
        <div className="calculator">
          <Formula formula={this.state.formula.replace(/x/g, '*')} />
          <Output currentValue={this.state.currentVal} />
          <Buttons
            decimal={this.handleDecimal}
            evaluate={this.handleEvaluate}
            initialize={this.initialize}
            numbers={this.handleNumbers}
            operators={this.handleOperators}
          />
        </div>
        <div className="author">
          {' '}
          Designed and Coded By <br />
          <a href="https://goo.gl/6NNLMG" target="_blank">
            JARIFUL HAQUE MALLICK
          </a>
        </div>
      </div>
    );
  }
}

class Buttons extends React.Component {
  render() {
    return (
      <div>
        <button
          className="jumbo"
          id="clear"
          onClick={this.props.initialize}
          style={clearStyle}
          value="AC"
        >
          AC
        </button>
        <button
          id="divide"
          onClick={this.props.operators}
          style={operatorStyle}
          value="/"
        >
          /
        </button>
        <button
          id="multiply"
          onClick={this.props.operators}
          style={operatorStyle}
          value="x"
        >
          x
        </button>
        <button id="seven" onClick={this.props.numbers} value="7">
          7
        </button>
        <button id="eight" onClick={this.props.numbers} value="8">
          8
        </button>
        <button id="nine" onClick={this.props.numbers} value="9">
          9
        </button>
        <button
          id="subtract"
          onClick={this.props.operators}
          style={operatorStyle}
          value="‑"
        >
          ‑
        </button>
        <button id="four" onClick={this.props.numbers} value="4">
          4
        </button>
        <button id="five" onClick={this.props.numbers} value="5">
          5
        </button>
        <button id="six" onClick={this.props.numbers} value="6">
          6
        </button>
        <button
          id="add"
          onClick={this.props.operators}
          style={operatorStyle}
          value="+"
        >
          +
        </button>
        <button id="one" onClick={this.props.numbers} value="1">
          1
        </button>
        <button id="two" onClick={this.props.numbers} value="2">
          2
        </button>
        <button id="three" onClick={this.props.numbers} value="3">
          3
        </button>
        <button
          className="jumbo"
          id="zero"
          onClick={this.props.numbers}
          value="0"
        >
          0
        </button>
        <button id="decimal" onClick={this.props.decimal} value=".">
          .
        </button>
        <button
          id="equals"
          onClick={this.props.evaluate}
          style={equalsStyle}
          value="="
        >
          =
        </button>
      </div>
    );
  }
}

class Output extends React.Component {
  render() {
    return (
      <div className="outputScreen" id="display">
        {this.props.currentValue}
      </div>
    );
  }
}

class Formula extends React.Component {
  render() {
    return <div className="formulaScreen">{this.props.formula}</div>;
  }
}

ReactDOM.render(<Calculator />, document.getElementById('app'));
    </script>
</body>
</html>
