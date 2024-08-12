# KodeDraw

KodeDraw is a Python library designed to help you easily create, layout, and render diagrams programmatically. Using Pydantic for data modeling, it supports complex diagrams with shapes, containers, and connections, while providing automatic layout algorithms to organize diagram elements.

## Features

- **Automatic Layouts**: Automatically organize diagram elements using customizable layout algorithms.
- **Customizable Styles**: Apply and merge styles to shapes and containers for a consistent look.
- **Context Management**: Use context variables to manage nested structures and maintain global context.
- **Diagram Export**: It can be expanded to export diagrams to various formats, it includes the Draw.io compatible XML rendering.

## Installation

You can install KodeDraw using pip 
(when I am able to publish to it, **not yet**):

```bash
#pip install kodedraw
```

## Usage

### Basic Example

```python
from kodedraw import Util, Diagram, Group, Shape, Edge, Style

# Define styles
shp_style = Style(
    rounded="1", 
    whiteSpace="wrap", 
    fillColor="#D3D3D3"
)

def diagrams_to_code():
    stencils = ['aws4']
    themes = ['light']
    with Util.get_model(stencils, themes) as diagrams_model,\
         Diagram(name="Page-1"):
            with Group(text='Shared Services Account') as ss_account:
                Shape("cloudformation", text='Account<br>Baseline<br>')
                Shape("vpc", text='Amazon VPC', style=shp_style)
            with Group(text='Log Archive Account') as log_archive_account:
                Shape("cloudformation", text='Account<br>Baseline<br>')
                Shape("flow_log", text='Aggregate<br>CloudTrail<br>and Config Logs<br>')
            with Group(text='Security Account') as security_account:
                Shape("cloudformation", text='Account<br>Baseline<br>')
                Shape("addon", text='Security<br>Cross-Account<br>Roles<br>')
                Shape("guardduty", text='Amazon<br>GuardDuty<br>')
                Shape("sns", text='Amazon SNS')
            with Group(text='AWS Cloud'):
                account_baseline = Shape("cloudformation", text='Account<br>Baseline<br>')
                code_pipeline = Shape("codepipeline", text='AWS<br>CodePipeline<br>')
                s3_bucket = Shape("bucket", text='Amazon S3<br>Bucket<br>')
                organizations = Shape("organizations", text='AWS<br>Organizations<br>')
                sso = Shape("cloudwatch", text='AWS Single<br>Sign-on<br>')
                service_catalog = Shape("service_catalog", text='AWS Service<br>Catalog<br>')
                parameter_store = Shape("parameter_store", text='AWS<br>Parameter<br>Store<br>')
                core_ou = Shape("resources", text='Core OU')
                
            core_ou >> [ss_account, 
                        log_archive_account, 
                        security_account] 
            organizations>>[sso,core_ou]
            s3_bucket >> \
                code_pipeline >> Edge(text="flow", style=Style(strokeColor="blue")) >>\
                    [organizations,
                    account_baseline,
                    service_catalog,
                    parameter_store] 

    return diagrams_model


if __name__ == '__main__':
    diagrams_model=diagrams_to_code()    # Create and draw the diagram from the model
    Util.to_drawio(model=diagrams_model,
                        auto_layout=True,
                        output_drawio_path='./output/generated.drawio',
                        title_shape=True)
```

## Documentation

### Models

- **Diagrams**: The root model that holds multiple diagrams.
- **Diagram**: Represents a single diagram page.
- **Group**: Represents a group that can hold multiple shapes or other containers. (Groups are containers with visual icons)
- **Shape**: Represents a single shape in the diagram.
- **Edge**: Represents a edge between shapes (Shapes are nodes with visual shapes).
- **Style**: Represents the styling attributes for shapes and containers (BaseStyle with common attributes).
- **LayoutStyle**: Defines the parameters for layout algorithms.

### Layout Algorithms

- **DefaultLayoutAlgorithm**: The default layout algorithm to automatically arrange diagram elements.
(more layouts algorithms to come)

## License

This project is licensed under the MYT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please read the [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to this project.

## Acknowledgements

Special thanks to all contributors and the open-source community for making this project possible.

## Contact

For questions or comments, please open an issue on GitHub.
