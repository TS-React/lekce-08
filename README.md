# Lekce 8: Načítání dat ze serveru

V dnešní lekci si ukážeme, jak lépe pracovat s daty získanými ze servery.

Doposud jsme řešili načítání dat obvykle uvnitř `useEffect` přímo v komponentě, která data potřebovala.

```tsx
const [users, setUsers] = useState(null);

useEffect(() => {
  async function loadUsers() {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    const data = await res.json();
    setUsers(data);
  }
  loadUsers();
}, []);
```

To sice není nutně špatně, ale zároveň je to nepraktické. Výše je pouze primitivní příklad, kde neřešíme žádné potenciální chybové stavy, přerušení dotazu v případě potřeby, apod. V reálné aplikaci by byl kód **mnohem** složitější. A navíc bychom ho v každé komponentě, která potřebuje data, v podstatě kompletně duplikovali, pouze s jinou adresou API endpointu.

Takový komplexní kód nám komponenty extrémně znepřehledňuje. Komponenta je plná "mechanické práce" s načítáním dat, místo aby se starala jen o UI a zobrazení dat.

Celý problém můžeme vyřešit tak, že si vyrobíme vlastní hook, do kterého logiku vyčleníme.

## Vlastní hook `useFetch`

Naším cílem je napsat si vlastní hook, do kterého přesuneme celou logiku komunikace se serverem, ošetření chyb, apod. Chceme do hooku vždy jen předat adresu, odkud chceme data načíst, a hook by nám měl vrátit tři hodnoty:

- `data` bude proměnná, která nakonec bude obsahovat získaná data
- `error` bude obsahovat případnou chybu
- `isLoading` bude obsahovat `true`/`false` podle toho, zda se data načítají
