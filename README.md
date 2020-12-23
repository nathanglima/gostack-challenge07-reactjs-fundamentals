<img alt="GoStack" src="https://camo.githubusercontent.com/a869a2aaab296ef925343d7e76518cd213eb0a30/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f676f6c64656e2d77696e642f626f6f7463616d702d676f737461636b2f6865616465722d6465736166696f732d6e65772e706e67" />

<h2 align="center">
  Challenge 07: ReactJS Fundamentals
</h2>


## :page_facing_up: About this challenge

This challenge, we were challenged to consuming the API developing in previous challenge, listing and upload transactions file using a PostgreSQL database.

## :rocket: About of tests
Some rules for evaluating this challenge were proposed, some tests were written using jest and to pass the tests it is necessary to respect all the points below:

---

#### *should be able to list the total balance inside the cards*

In order for this test to pass, your application must allow show in your Dashboard: cards having total of income, outcome and total subtraction of (income - outcome) that are returned for balance from your back-end.

```js
<CardContainer>
  <Card>
    <header>
      <p>Entradas</p>
      <img src={income} alt="Income" />
    </header>
    <h1 data-testid="balance-income">{formatValue(balance.income)}</h1>
  </Card>
  <Card>
    <header>
      <p>Saídas</p>
      <img src={outcome} alt="Outcome" />
    </header>
    <h1 data-testid="balance-outcome">{formatValue(balance.outcome)}</h1>
  </Card>
  <Card total>
    <header>
      <p>Total</p>
      <img src={total} alt="Total" />
    </header>
    <h1 data-testid="balance-total">{formatValue(balance.total)}</h1>
  </Card>
</CardContainer>
```

#### *should be able to list the transactions*

In order for this test to pass, your application must allow listing into of a table, all transactions that are returned from your back-end.

```js
<TableContainer>
  <table>
    <thead>
      <tr>
        <th>Título</th>
        <th>Preço</th>
        <th>Categoria</th>
        <th>Data</th>
      </tr>
    </thead>

    <tbody>
      {transactions.map((transaction) => (
        <tr key={transaction.id}>
          <td className="title">{transaction.title}</td>
          <td className={transaction.type}>
            {transaction.type == 'outcome' ? ' - ' : null}
            {formatValue(transaction.value)}</td>
          <td>{transaction.category.title}</td>
          <td>{format(new Date(transaction.created_at), 'dd/MM/yyyy')}</td>
        </tr>
      ))}
    </tbody>
  </table>
</TableContainer>
```

#### *should be able to navigate to the import page*

In order for this test to pass, you must allow the page switch through the Header, by button with name "Importar".

```js
<Container size={size}>
  <header>
    <img src={Logo} alt="GoFinances" />
    <nav>
      {
        <>
          <Link to="/">Listagem</Link>
          <Link to="/import">Importar</Link>
        </>
      }
    </nav>
  </header>
</Container>
```

#### *should be able to upload a file*

In order for this test to pass, you must allow that file be sent through the component of drag-n-drop of import page and that is possible show the name of file sending through the input text.

```js
const [uploadedFiles, setUploadedFiles] = useState<FileProps[]>([]);
const history = useHistory();

async function handleUpload(): Promise<void> {
  const data = new FormData();

  if(!uploadedFiles.length) return;

  const file = uploadedFiles[0];

  data.append('file', file.file, file.name);

  try {
    await api.post('/transactions/import', data);
    history.push('/');

  } catch (err) {
    console.log(err.response.error);
  }
}

function submitFile(files: File[]): void {
  const upload = files.map(file => ({
    file,
    name: file.name,
    readableSize: filesize(file.size),
  }));
  
  setUploadedFiles(upload);
}
```

```js
<ImportFileContainer>
  <Upload onUpload={submitFile} />
  {!!uploadedFiles.length && <FileList files={uploadedFiles} />}

  <Footer>
    <p>
      <img src={alert} alt="Alert" />
      Permitido apenas arquivos CSV
    </p>
    <button onClick={handleUpload} type="button">
      Enviar
    </button>
  </Footer>
</ImportFileContainer>
```
