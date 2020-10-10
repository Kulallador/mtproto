# Получение access_hash

Telgram перекладывает вопросы доступа на клиенты, и это никак не изменишь.

Для некоторых запросов нужно сделать InputUser, InputChannel, InputMedia нужен специальный параметр Access hash. Документация не указывает ни как он устроен, ни как его четко получить. Эта статья описывает найденные способы получения access_hash

Судя по всему, access_hash является хэшем от сочетания id пользователя, id объекта, к которому относится хеш и ключом, который есть у серверов telegram, но неизвестно ничего ни про алгоритм хеша, ни про то, что конкретно хешируется. Возможно ответ есть в [MTProxy](https://github.com/TelegramMessenger/MTProxy), но четкого понимания, где искать способ получений хеша непонятно. Поэтому эта статья пытается описать, как можно это значение получить без копания в внутренней структуре telegram.

[Один из ответов в Stack overflow](https://stackoverflow.com/questions/46736549/telegram-channel-how-to-get-access-hash)

## Способы оптимизации

Один из вариантов, как не тратить время на поиск хеша — кешировать в базе вроде redis или memcached. Обратите внимание, что для **РАЗНЫХ** аккаунтов **РАЗНЫЕ** `access_hash`, поэтому рекомендуется либо получать публичную информацию с нескольких _главных_ аккаунтов (с которых вы собираете информацию), и хранить ее в вашем хранилище.

## способы получения хэшей

Список способов, скорее всего не окончательный, если у вас есть доп информация, либо вы нашли новый способ, не указанный в этой статье, создайте PR, с описанием.

### InputUser

#### резолв по никнейму

contacts.resolveUsername() -> Contacts.ResolvedPeer

Contacts.ResolvedPeer.users содержит слайс из 1 пользователя, по которому произошел резолв. User (user constructor) хранит в себе id и access_hash

### InputChannel

#### резолв по инвайт сслыке