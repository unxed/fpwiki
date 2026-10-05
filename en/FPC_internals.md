# FPC internals

The basic parts of this documentation are taken from the the 1.0.x internals documentation ([[1]](<ftp://ftp.freepascal.org/fpc/docs-pdf/comparch.pdf>)) written by Carl-Eric Codere. They are adapted to fit the changed parts of 1.9.x. This documentation is still under construction. 

  1. [Introduction](<Introduction.md> "Introduction")
  2. [Scanner/Tokenizer](<Scanner/Tokenizer.md> "Scanner/Tokenizer")
  3. [The parse tree](<The_parse_tree.md> "The parse tree")
  4. [Symbol tables](<Symbol_tables.md> "Symbol tables")
  5. [Symbol entries](<Symbol_entries.md> "Symbol entries")
  6. [Type information](<Type_information.md> "Type information")
  7. [The parser](<The_parser.md> "The parser")
  8. [The inline assembler parser](<The_inline_assembler_parser.md> "The inline assembler parser")
  9. [The code generator](<The_code_generator.md> "The code generator")
     1. [Node code generator](</index.php?title=Node_code_generator&action=edit&redlink=1> "Node code generator \(page does not exist\)")
     2. [Code generator abstraction layer](</index.php?title=Code_generator_abstraction_layer&action=edit&redlink=1> "Code generator abstraction layer \(page does not exist\)")
     3. [The register allocator](<The_register_allocator.md> "The register allocator")
  10. [The optimizer](</index.php?title=The_optimizer&action=edit&redlink=1> "The optimizer \(page does not exist\)")
  11. [The assembler output](<The_assembler_output.md> "The assembler output")
  12. [Generating initialised data](<Generating_initialised_data.md> "Generating initialised data")
     * [Layout of certain compiler-generated data and data structures](<Compiler-generated_data_and_data_structures.md> "Compiler-generated data and data structures")
  13. [Message files](<Message_files.md> "Message files")

---

_Source: [https://wiki.freepascal.org/FPC_internals](https://web.archive.org/web/20210125101625/https://wiki.freepascal.org/FPC_internals)_
