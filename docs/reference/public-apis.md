# Справочник: публичные API для трейдинг-агентов

Курированная выжимка из [`public-apis/public-apis`](https://github.com/public-apis/public-apis)
(MIT license, © public-apis contributors) — открытого списка из тысяч публичных
API по всем доменам. Сам репозиторий — не Claude-скилл/плагин/MCP-сервер, а
чистый markdown-справочник; ставить через `claude plugin install` там нечего
(разбор см. в истории установок стороннего кода — прошёл проверку и признан
безопасным как справочный материал, не как исполняемый код).

Ниже — только 4 категории, релевantные трейдинг-фокусу этого репо (полный
список — 60+ категорий, см. оригинал). Задача файла: дать агентам
(`data-engineer`, `crypto-trader`, `order-flow-analyst`, `onchain-sentiment-analyst`)
локальную отправную точку без похода в сеть за каждым именем API. Актуальность
эндпоинтов, лимитов и авторизации проверяйте на сайте каждого сервиса —
список меняется быстрее, чем этот файл будет обновляться.

Легенда: **без ключа** — можно дёргать сразу; **apiKey** — нужна регистрация и
API-ключ (обычно есть бесплатный тир); **OAuth** — полноценный OAuth-флоу
(банковские/брокерские интеграции).

## Биржи и рыночные данные (крипто)

Основной источник OHLCV/стакана/сделок для бэктестов и live-контекста.

- **Без ключа, для быстрого старта**: CoinGecko (цены/маркеты/dev-соц-данные),
  CoinCap, Coinpaprika, Coinlore, CryptoCompare, Mempool.space (комиссии/mempool
  BTC), Gemini, Localbitcoins.
- **apiKey, но щедрый бесплатный тир**: Binance, Bybit, KuCoin, OKEx, Kraken,
  Coinbase / Coinbase Pro, Bitfinex, Bitmex, Huobi, Gateio, Mexc, CoinMarketCap,
  Messari, Nomics, CryptoMarket, EXMO, VALR, NovaDax, BtcTurk, Indodax.
- **Деривативы / funding & OI** (прямое пересечение с `order-flow-analyst`):
  Bybit, Bitmex, Binance, Hyperliquid Market Data (funding + OI + predicted
  rates по монетам, через RapidAPI).
- **DEX / on-chain торговля**: 0x, 1inch, dYdX (декентрализованная биржа),
  BlazePhoenix (DEX-агрегатор котировок).

## Блокчейн / он-чейн данные

Для `onchain-sentiment-analyst` и `order-flow-analyst` (крупные транзакции,
активность сети, ончейн-метрики).

- **Explorers / node providers**: Etherscan (Ethereum), Blockscout
  (мультичейн, Etherscan-совместимый JSON-RPC), Covalent (мультичейн
  агрегатор), Alchemy Ethereum, INFURA Ethereum (node-as-a-service, apiKey),
  Solana JSON RPC (без ключа), blockfrost (Cardano).
- **Индексация / графовые запросы**: The Graph (GraphQL по Ethereum и др.),
  Bitquery (GraphQL on-chain + DEX).
- **Специфика BTC**: Mempool.space (без ключа), Blockchain.com API, Block.io
  (платежи/кошельки).
- **Прочее**: Chainlink (оракулы для смарт-контрактов), Ethplorer (токены/
  балансы/история транзакций ETH).

## Валютный рынок (FX) — курсы и конвертация

Полезно для мульти-валютных бэктестов и когда котировки нужны не только в USD.

- **Без ключа**: Exchangerate.host, Frankfurter (курсы + временные ряды),
  FreeForexAPI, Currency-api (150+ валют, без лимитов), Bank of Russia
  (официальный ЦБ РФ XML), Czech National Bank, National Bank of Poland.
- **apiKey**: Currencylayer, Exchangeratesapi.io, Fixer, 1Forge (форекс-данные),
  CurrencyFreaks, CurrencyScoop, ExchangeRate-API.

## Финансы / традиционные рынки (акции, макро, отчётность)

Для контекста, коррелирующего с крипторынком (DXY-прокси через FX выше,
макро-данные, риск-он/риск-офф фон).

- **Макро**: FRED (Federal Reserve Bank of St. Louis, apiKey), Econdb
  (глобальная макростатистика, без ключа), Fed Treasury (US Treasury Fiscal
  Data, без ключа).
- **Акции / рыночные данные**: Alpha Vantage, Twelve Data, Finnhub
  (стоки + FX + крипто в одном API), IEX Cloud, Polygon, Marketstack,
  Financial Modeling Prep, Yahoo Finance (apiKey, несмотря на «free»-репутацию).
- **Регуляторка / инсайдеры**: SEC EDGAR (без ключа, годовые отчёты публичных
  компаний США), Aletheia (инсайдерская торговля + анализ звонков по доходам).

## Как пользоваться этим файлом

1. Перед тем как `data-engineer` пишет новый загрузчик данных — сначала
   проверить, нет ли готового источника здесь (не изобретать парсер под сайт,
   если есть API).
2. Перед добавлением сюда нового API — убедиться, что он из исходного списка
   `public-apis/public-apis` или лично проверен; не копировать чужие таблицы
   бездумно, описывать своими словами и с пометкой относительно трейдинг-контекста
   этого репо.
3. Полный список (60+ категорий, тысячи API) — в оригинале:
   <https://github.com/public-apis/public-apis>.

---
Источник: [`public-apis/public-apis`](https://github.com/public-apis/public-apis),
лицензия MIT. Это курированная авторская выжимка (не копия таблиц), актуальность
по состоянию на дату добавления в этот репозиторий.
