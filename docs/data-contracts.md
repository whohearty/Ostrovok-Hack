# Контракты и инварианты · draft
| Сущность | Минимальные поля и правила |
|---|---|
| TripRequest | origin, destination или flexible_destination, date_range, nights, adults, child_ages, pets, budget_amount_minor, budget_scope, currency, preferences, hard_constraints, assumptions |
| Offer | id, provider, data_kind(real/synthetic), source_url/reference, retrieved_at, valid_until/unknown, location, local_timezone, price_minor, price_basis, party_coverage, cancellation, attributes |
| Transport | departure/arrival с timezone, stations, baggage, transfer_time, passenger coverage, ограничения |
| Accommodation | check_in/out с timezone, occupancy, питание, дети, питомцы, сборы, refundable_until и штраф |
| CandidateTrip | component_ids, total_price_minor, price_breakdown, hard_constraint_results, preference_score, risks, evidence, missing_information |
| TripState | session_id, version, confirmed_constraints, assumptions, chosen_components, invalidated_components, cart_version, events |
| MockOrder | order_id, expected_state_version, idempotency_key, components, total, confirmed_at, is_simulated=true |

Деньги — целое число минимальных денежных единиц. Стоимость сессии и экономика могут использовать Decimal. Цена пакета — транспорт для всей группы + все ночи проживания + нужные трансферы + известные обязательные сборы. Неизвестные доплаты показываются отдельно; отсутствие суммы не равно нулю. Не смешиваем разные валюты без явного курса/даты; для MVP предпочтительна одна валюта.

Время хранится с timezone и выводится локально; порядок событий сверяется в единой шкале. Буферы аэропорта, переезда и заселения — именованные параметры с происхождением, не универсальная гарантия. Ночной прилёт может потребовать предыдущую ночь или подтверждение раннего заселения. Отмена/питомец/возраст ребёнка — проверяемые ограничения, а не текстовая близость.

Если бюджет неоднозначен «на человека/всех» — уточнить. Если дата частичная, сохранять исходную формулировку и показать интерпретацию до покупки. Модель не может создать offer_id, цену или новое свойство вне ответа адаптера. Объяснение ссылается на evidence полей и явно различает факты и предпочтения. Общие внешние рекомендации по визе, безопасности или медицине не превращаем в обещание допуска к поездке.
