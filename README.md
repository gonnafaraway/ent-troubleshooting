# ent-troubleshooting
Help guide for Entgo.io community (https://t.me/entgoru)

# edge "promotion" has a field "promotion_id" but it is not holding a foreign key (https://github.com/ent/ent/issues/1417)
* Это особенность ent, когда нет нормальной возможности сделать O2O, в таком случае технически проще описать схему как M2M без ключей Unique у полей в schema и только в этом случае ошибки не будет, иначе сущности со связями будут уникальными и создать вторую и более связь будет невозможно
# Error: loading schema: entc/load: no schema found in: (https://github.com/ent/ent/issues/3864 or another)
* На данный момент ent не поддерживает go версии 1.22, с высокой долей вероятности у вас либо сам компилятор версии 1.22, либо проект. Откатитесь до 1.21.7 и все заработает :)
