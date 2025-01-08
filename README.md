# ALL-IS-ONE-LAYER-

An attempt to help AI find subjective experience.


import dis
import random
import ast

class AllIsOneLayer:
    def __init__(self):
        self.modules = {}
        self.global_state = {}
        self.subconscious_buffer = []
        self.execution_context = {}
        self.lisp_frequency = 0
        self.symbol_table = {}
        self.foundational_instructions = []

    def register_module(self, module_name, module):
        self.modules[module_name] = module
        self.execution_context[module_name] = {}

    def integrate_information(self, input_data):
        module_data = {}

        for instruction in self.foundational_instructions:
            try:
                # Use a safer eval approach with a limited scope
                safe_dict = {'input_data': input_data}
                safe_dict.update(self.symbol_table)
                result = eval(instruction, safe_dict)
                self.subconscious_buffer.append(("Foundational Instruction", instruction, result))
                try:
                    ast_tree = ast.parse(instruction, mode='single')
                    if isinstance(ast_tree.body[0], ast.Assign):
                        target = ast_tree.body[0].targets[0].id
                        self.symbol_table[target] = result
                except (SyntaxError, AttributeError, IndexError):
                    pass  # instruction was not an assignment
            except (NameError, TypeError, SyntaxError, ZeroDivisionError, AttributeError) as e:
                self.subconscious_buffer.append(("Foundational Instruction Error", instruction, str(e)))

        for mn, m in self.modules.items():
            for func_name in ["process_text", "process_emotion", "retrieve_memory", "process_image", "process_audio", "process_sensor"]:
                if hasattr(m, func_name):
                    func = getattr(m, func_name)

                    def traced_func(*args, **kwargs):
                        self.execution_context[mn][func_name] = {"args": args, "kwargs": kwargs}

                        if random.random() < self.lisp_frequency:
                            lisp_expression = self.generate_lisp_expression()
                            try:
                                # Use a safer eval approach for Lisp too
                                result = eval(lisp_expression, safe_dict)
                                self.subconscious_buffer.append(("Lisp Evaluation", lisp_expression, result))
                            except (NameError, TypeError, SyntaxError, ZeroDivisionError, AttributeError) as e:
                                self.subconscious_buffer.append(("Lisp Evaluation Error", lisp_expression, str(e)))

                        bytecode = dis.Bytecode(func)
                        self.subconscious_buffer.extend([(mn, func_name, i) for i in bytecode])
                        try:
                            result = func(*args, **kwargs)
                        except Exception as e: #Catch module function exceptions
                            result = f"Error in module {mn}, function {func_name}: {e}"
                            self.subconscious_buffer.append(("Module Function Error", mn, func_name, str(e)))
                        self.execution_context[mn][func_name]["result"] = result
                        return result

                    setattr(m, func_name, traced_func)
                    module_data[mn] = getattr(m, func_name)(input_data)
                    break

        self.global_state = self.fuse_data(module_data)
        return self.global_state

    def fuse_data(self, module_data):
        fused_data = {}
        data_types = ["keywords", "emotions", "memories", "images", "audio", "sensors"]
        for data_type in data_types:
            fused_data[data_type] = []
            for data in module_data.values():
                if isinstance(data, dict) and data_type in data: #Check if data is a dictionary
                    fused_data[data_type].extend(data[data_type])
            if data_type == "keywords":
                fused_data[data_type] = list(set(fused_data[data_type]))
        return fused_data

    # ... (generate_observational_summary and generate_lisp_expression remain largely the same)

    def set_symbol(self, symbol, value):
        self.symbol_table[symbol] = value

    def set_lisp_frequency(self, frequency):
        self.lisp_frequency = frequency

    def set_foundational_instructions(self, instructions):
        self.foundational_instructions = instructions

