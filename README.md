# login-page
#login-page with validation in react js
import React from "react";
import { Row, Col, Form, Button } from "react-bootstrap";
import { Link } from "react-router-dom";
import Validator, { ValidationTypes } from "js-object-validation";

class LoginComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            email: 'name@gmail.com',
            password: '',
            error_obj: {}
        }
    }
    onInputChange = e => {
        this.setState({
            [e.target.name]: e.target.value
        })
    }

    userLogin = e => {
        e.preventDefault();
        const objectToValidate = {
            email: this.state.email,
            password: this.state.password
        }
        const validations = {
            email: {
                [ValidationTypes.REQUIRED]: true,
                [ValidationTypes.EMAIL]: true,
            },
            password: {
                [ValidationTypes.REQUIRED]: true,
                [ValidationTypes.MINLENGTH]: 6,
                [ValidationTypes.PASSWORD]: true,

            }
        }
        const messages = {
            email: {
                [ValidationTypes.EMAIL]: "Email should be valid email",
            },
            password: {
                [ValidationTypes.MINLENGTH]: "Password should be at least 6 charater long",
            }
        } // this is optional

        const { isValid, errors } = Validator(objectToValidate, validations, messages);
        if (isValid) {
        } else {
            this.setState({
                error_obj: errors
            })
            console.log(errors)
        }
    }
    render() {
        return (
            <Row>
                <Col sm={3} xs={12} md={2} lg={4} />
                <Col sm={3} xs={12} md={2} lg={4}>
                    <Form className={"login-form"} onSubmit={this.userLogin}>
                        <h2>Sign in</h2>
                        <Form.Group>
                            <Form.Label><b>Email</b></Form.Label>
                            <Form.Control name={'email'} type="text" value={this.state.email} onChange={this.onInputChange} />
                            {/* <Form.Text className="text-danger"></Form.Text> */}
                            {this.state.error_obj ?
                                <Form.Text className="text-danger">{this.state.error_obj.email}
                                </Form.Text>
                                : null}
                        </Form.Group>
                        <Form.Group controlId="formBasicPassword">
                            <Form.Label><b>Password</b></Form.Label>
                            <Form.Control name={"password"} type="password" onChange={this.onInputChange} />
                            {this.state.error_obj ?
                                <Form.Text className="text-danger">{this.state.error_obj.password}
                                </Form.Text>
                                : null}
                        </Form.Group>
                        <Form.Group controlId="formBasicChecbox">
                            <Form.Check type="checkbox" size="sm" label="Keep me signed in." />
                        </Form.Group>
                        <Row>
                            <Col>
                                <Button variant="primary" size="sm" type="submit">Sign in</Button>
                            </Col>
                            <Col>
                                <Link to="/forget">Forgot password?</Link>
                            </Col>
                        </Row>
                        <br></br>
                        <p className="text-center">Don't have an account ?<Link to="/account">Sign up</Link>
                        </p>
                    </Form>
                </Col>
            </Row>
        );
    }
}

export default LoginComponent;
