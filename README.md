# Prometheus measures exporter

## Start http service for export metrics
```rust
start_prometheus_exporter();
```
## Measure
```rust
let counter = create_and_register_measurer::<IntCounter, _>(
         &prometheus::default_registry(),
         Opts::new("name", "help").const_labels(
             vec![("label".to_string(), "value".to_string())]
                 .into_iter()
                 .collect(),
         ) 
);
counter.inc();
```

## Measures by labels
```rust

let mut measures = MeasurersByLabel::<String, IntCounter, Opts>::new(
    &prometheus::default_registry(),
    Box::new(|key| {
                Opts::new("name", "help").const_labels(
                    vec![("label".to_string(), key.to_string())]
                        .into_iter()
                        .collect(),
                )
            }),
        );
let counter = measures.measurer("some_measure".to_owned());
counter.inc();
```
