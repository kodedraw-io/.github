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
from kodedraw.render import KodeDraw
from kodedraw.models import Diagrams, Diagram, Container, Shape, Connection, Style, LayoutStyle, DefaultLayoutAlgorithm

# Define styles
diagram_style = Style(
    pageWidth="827", 
    pageHeight="1169", 
    dx="1394", 
    dy="850"
)
cnt_style = Style(
    swimlane="1", 
    fillColor="#FFFFE0"
)
shp_style = Style(
    rounded="1", 
    whiteSpace="wrap", 
    fillColor="#D3D3D3"
)
cnx_style = Style(
    edgeStyle="orthogonalEdgeStyle", 
    rounded="0", 
    orthogonalLoop="1", 
    jettySize="auto"
)
cnt_title_style = Style(
    fontColor="#FFFFFF",
    fillColor="#000000",
    align="center",
    verticalAlign="top",
    whiteSpace="wrap",
)

layout_style = LayoutStyle(spacing_x=30, spacing_y=30, equal_size=True, direction="LR")
layout_algorithm = DefaultLayoutAlgorithm(layout_style)

# Create a high-level diagram model
with Diagrams(layout_style=layout_style) as diagram_model:
    with Diagram(name="First Page", connection_style=cnx_style, diagram_style=diagram_style):
        with Container(
            title_text="Front End Platform",
            title_style=cnt_title_style,
            style=cnt_style, width=400, height=200
        ) as fe:
            s1 = Shape(text="deco", 
                       style=shp_style, width=100, height=40)
            s2 = Shape(text="Integração Magazine", 
                       style=shp_style, width=100, height=30)
            s3 = Shape(text="Integração Ágil", 
                       style=shp_style, width=100, height=30)
            
        with Container(
            title_text="Commerce Platform",
            style=cnt_style, width=400, height=600
        ) as commerce:
            s4 = Shape(text="Magento", 
                       style=shp_style, width=100, height=40)
            s5 = Shape(text="Pick up store", 
                       style=shp_style, width=100, height=30)
            s6 = Shape(text="Kiosk", 
                       style=shp_style, width=100, height=30)

        # Apply layout algorithm to the diagram
        layout_algorithm.apply_on_diagram(diagram_model)

        # Establish connections with chaining and styles
        s1 >> s4
        s2 >> Connection(style=Style(strokeColor="blue")) >> s5
        s3 >> s6
        fe >> commerce

    with Diagram(name="Second Page", connection_style=cnx_style, diagram_style=diagram_style) as second_page:
        with Container(
            title_text="Another Container",
            style=cnt_style, width=400, height=200
        ):
            s7 = Shape(text="Another Shape", 
                       style=shp_style, width=100, height=40)

        # Apply layout algorithm to the diagram
        layout_algorithm.apply_on_diagram(diagram_model)

# Create and draw the diagram from the model
diagram = KodeDraw(model=diagram_model).draw()

# Save the diagram to a file
diagram.save('example.drawio')

# Dump model to JSON
print(diagram_model.model_dump_json(indent=2))
```

## Documentation

### Models

- **Diagrams**: The root model that holds multiple diagrams.
- **Diagram**: Represents a single diagram page.
- **Container**: Represents a container that can hold multiple shapes or other containers.
- **Shape**: Represents a single shape in the diagram.
- **Connection**: Represents a connection between shapes.
- **Style**: Represents the styling attributes for shapes and containers.
- **LayoutStyle**: Defines the parameters for layout algorithms.

### Layout Algorithms

- **DefaultLayoutAlgorithm**: The default layout algorithm to automatically arrange diagram elements.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please read the [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to this project.

## Acknowledgements

Special thanks to all contributors and the open-source community for making this project possible.

## Contact

For questions or comments, please open an issue on GitHub.
